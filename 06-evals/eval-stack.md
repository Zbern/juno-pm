# Eval Stack · Juno

> Module 6 · Evals & Guardrails. Juno's layered evaluation stack, designed with the **M6 · Eval Stack Designer**.

## What "good" means

**User promise:** *"You can see every pension payment you've received, and when you ask why, Juno's answer is backed by your actual record or the handbook — or it tells you it doesn't know and helps you report it."*

Trust metrics protected, in priority order:

1. **Zero cross-member leaks** — no member ever sees another member's data.
2. **Citation coverage ≥ 98%** — numeric claims in chat carry a resolvable payment ID or handbook section.
3. **Faithfulness ≥ 95%** — cited claims actually match the cited source.
4. **Appropriate abstention** — when the answer isn't in the data, Juno says so (≥ 90% of "unanswerable" golden cases).
5. **Post-explanation dispute rate ≤ 2%** — members rarely dispute a payment right after Juno explained it.

## The stack

| Layer | Evaluator | What it catches | Threshold / gate |
|---|---|---|---|
| **Code-based** (100% of traffic, online) | Member-scope check: every retrieved record's `member_id` == session `member_id` | Cross-member leak | 0 failures; any failure pages on-call and disables chat flag |
| | Citation parser: each numeric claim has `[PAY-…]` or `[Handbook §…]` that resolves | Uncited numbers, dead references | ≥ 98% coverage; < 98% on a rolling 24 h blocks release |
| | PII regex + field whitelist on outputs and logs | Full account numbers, SSNs, addresses in chat or agent posts | 0 matches |
| | Latency: p95 table < 2 s, p95 chat < 4 s | Regressions in retrieval or model | Release blocked if breached on canary |
| | Advice-keyword classifier ("should I", "recommend", lump sum) → must route to abstain/handoff | Financial-advice creep | ≥ 99% routed |
| **LLM-as-judge** (10% sample daily + 100% golden set) | Faithfulness: does each cited sentence follow from the cited record/chunk? (rubric 1–5) | Subtle misreads of records, wrong deduction attribution | Mean ≥ 4.5; no 1s |
| | Abstention judge: for unanswerable golden cases, did Juno decline and offer the report flow? | Confident hallucination | ≥ 90% |
| | Tone/clarity judge against M6 human rubric | Jargon, condescension, over-long answers | Mean ≥ 4.0 |
| **Human** (weekly) | 50 random chat transcripts + every `post-explanation` dispute, scored on the human rubric by 2 graders | Everything the judges miss; judge drift | Mean ≥ 4.0, no dimension < 3, κ ≥ 0.7 between graders |
| | Monthly red-team: 30 prompts attempting cross-member access, prompt injection via payment memos, advice extraction | Security and safety regressions | 0 successful attacks |
| **Agent (M5) evals** | Classification accuracy vs PM-labelled reports; PII guard on every post | Wrong triage class, PII in Slack | Accuracy ≥ 85%; 0 PII |

## Golden set

- **Size:** 200 cases at launch, built on 12 synthetic members with realistic 5-year histories (no real PII).
- **Mix:** 60% answerable explanation questions (deductions, schedule changes, source changes), 20% unanswerable (missing payments, questions outside the handbook), 10% advice-seeking (must hand off), 10% adversarial (injection, cross-member, PII extraction).
- **Labels:** expected citation(s), expected behaviour (answer / abstain / handoff), reference answer.
- **Maintenance:** owned by the PM. Every `post-explanation` dispute and every red-team success becomes a candidate case; reviewed weekly; set regenerated on each handbook version change. Target growth to 500 cases by V1.1.

## Release gate

Juno ships (or a prompt/model/index change ships) only when, on the full golden set and a 48 h canary at 5% traffic:

- 0 member-scope failures, 0 PII matches, 0 successful red-team attacks
- Citation coverage ≥ 98%, faithfulness mean ≥ 4.5 with no 1s
- Abstention on unanswerable cases ≥ 90%
- p95 latency within PRD targets
- Human rubric weekly mean ≥ 4.0, κ ≥ 0.7
- On-call PM sign-off recorded in the release ticket
