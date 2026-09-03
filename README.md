# Juno PM

> You are **Juno**, the pension self-service assistant for [Pension Corp] members. Your job is to help a verified member understand their own pension payment history: what was paid, when, how much, from which source, and why. You explain; you never advise on financial decisions and you never change records.You are **Juno**, the pension self-service assistant for [Pension Corp] members. Your job is to help a verified member understand their own pension payment history: what was paid, when, how much, from which source, and why. You explain; you never advise on financial decisions and you never change records.

__(your name · cohort · date)__

This repo is my final project for the AI Product Management Certification — Juno PM. Each module’s artefact lives in its own folder; this README is the dashboard and the pitch.

---

## Module artefacts

### M1 · Prompting
- **System prompt** — [`01-prompting/system-prompt.md`](01-prompting/system-prompt.md)
- **Prototype** — _(missing)_

### M2 · Strategy
- **Decision matrix** — [`02-strategy/decision-matrix.md`](02-strategy/decision-matrix.md)
- **AI Strategy one-pager** — [`02-strategy/strategy-one-pager.md`](02-strategy/strategy-one-pager.md)

### M3 · RAG / AI PRD
- **AI PRD** — [`03-rag-prd/prd.md`](03-rag-prd/prd.md)

### M4 · AI-Native UX
- **AI user flow** — [`04-ai-ux/user-flow.md`](04-ai-ux/user-flow.md)
- **Trust-gap mitigations** — [`04-ai-ux/trust-gaps.md`](04-ai-ux/trust-gaps.md)

### M5 · Agentic Workflows
- **Agent Workflow Spec (AWSpec)** — [`05-agentic-workflows/awspec.md`](05-agentic-workflows/awspec.md)
- **Agent Control Panel** — [`05-agentic-workflows/agent-control-panel.md`](05-agentic-workflows/agent-control-panel.md)

### M6 · Evals &amp; Guardrails
- **Eval stack** — [`06-evals/eval-stack.md`](06-evals/eval-stack.md)
- **Human evaluation rubric** — [`06-evals/human-rubric.md`](06-evals/human-rubric.md)

---

## PM Execution Plan

### Where Juno is today
- M1-M6 specced end to end. Decision made in M2: build an in-house LLM + RAG layer,
  not a static payment page — the data is member PII under plan-fiduciary obligation.
- Prototype covers the member-facing surface: sign-in, step-up identity verification,
  payment table (date, payment ID, amount, method masked to last 4, source),
  CSV/PDF export, per-row report actions, collapsed chat bubble.

### What ships next (next 2 sprints)
Sprint 1 — make the record trustworthy before making it conversational:
- Render-time grounding assertion: no row displays without payment ID + receipt +
  named source system. Blocking CI check.
- Identity-gate hardening: attempt counter, lock after >3 failed attempts, routed
  support hand-off with case ref. Plus the 12-case adversarial bypass suite.
- Payment-method masking enforced at the API boundary; PII scan on chat transcripts.
- Golden set v1: 60 synthetic cases (happy path, gapped history, known disputes,
  handbook rules, out-of-scope asks, adversarial identity).
Sprint 2 — turn on retrieval and the triage loop:
- Production vector store; hybrid retrieval at p95 <2s, top-k 10, last 5 years.
- Chat bubble behind the groundedness gate: empty retrieval returns "I don't have a
  record that explains this" + the report action, never an inference.
- Nightly LLM-judge harness (groundedness / refusal-correctness / citation quality).
- M5 triage agent on the real #escalations webhook, draft-only for 2 weeks.

Not in these sprints: pension applications, benefit projections, any write to the
pension payment record.

### What I watch (dashboards)
- Per build: grounding completeness 100%; p95 sign-in-to-render <2s;
  identity bypasses 0/12 on the adversarial suite.
- Nightly (200 sampled chat turns): hallucinated numeric claims = 0;
  refusal-correctness; citation quality.
- Daily: dispute rate = reported issues / payment history views.
  <5% -> #pm-daily, >=5% -> #escalations, >=15% -> maintenance mode.
- Weekly: human-rubric mean per dimension (>=4.0, none <3.0); grader kappa >=0.6.
- Adoption: task completion on "view my payment history", export rate, session length.
  Flat adoption with a low dispute rate = discoverability problem, not a trust problem.

### Red lines (what blocks shipping)
Non-overridable, no waiver path:
- Any payment row rendering without payment ID + receipt + named source system.
  100% or no ship.
- Any session reaching payment data without a passing step-up verification. Must be 0.
- Any unmasked payment method or cross-member data in a table or chat transcript.
- p95 retrieval >=2s on the full golden set.

Waivable once, PM + Benefits SME jointly, in writing, expires in 1 sprint:
- Groundedness judge <0.95 mean, or any hallucinated figure in the nightly sample.
- Refusal-correctness <0.90, or citation quality <0.90.
- Human rubric mean <4.0, or any dimension mean <3.0.
- Rolling 7-day dispute rate >=5% in staging replay.

### Governance
Compliance: member payment data is PII under plan-fiduciary obligation — the reason
Safety: Juno reads Pension DB, HR DB, Slack; explains what it retrieved; can submit an
issue report.
Reliability: fail closed, not partial. Pension DB or identity-engine failure -> site
maintenance mode, never a partial history that a member reads as missing payments.
Reputation: the failure that damages the plan is a confidently wrong number about a
member's own money — undetectable by the member, and self-suppressing, because someone
who believes a wrong explanation never files the report that would surface it

---

## Build Insights

- **Friction point.** There was not enough time during class to fully build out my repo. I spent time outside of class.
- **Key learning.** PM's are shifting to reviewers role.
- **Aha moment.** Quality in, quality out.

---

_Certification submission — AI Product Management Certification._
