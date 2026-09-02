# Trust-Gap Mitigations · Juno

> Module 4 · AI-Native UX. Trust gaps surfaced with the **M4 · AI-UX Trust Gap Checker**, and how each is mitigated.

## Trust gaps

| Gap | Where it shows up | User cost | Mitigation |
|---|---|---|---|
| **Hallucination** | Chat invents a reason for a lower payment, or quotes a rule the handbook doesn't contain | Member acts on a false explanation; loses faith in *all* figures, including correct ones | Mandatory citation per numeric claim (PRD §4); abstain below 70% confidence; "Why this amount?" is scoped to one record so the model has little room to roam; M6 faithfulness eval gates release |
| **Opacity (no "why")** | A number changes month to month with no visible cause | Member assumes error, files a dispute or calls support | Every explanation ends with `[PAY-id]` / `[Handbook §x]` the member can click; receipts attached to every row; breadcrumbs show which source is being checked |
| **No user control** | Member disagrees with the explanation or doesn't want AI involvement | Feels trapped between "trust the bot" and "call support" | Persistent **Report** on every row; **Hide assistant** toggle; table and export work with the chat disabled |
| **Privacy anxiety** | Member wonders who else sees this and whether the AI "remembers" them | Avoids using the feature at all | Step-up verification before any personal data; session-bound member_id; visible note *"Juno only sees your records for this session and doesn't store your questions"*; payment methods masked |
| **Over-trust / advice creep** | Member asks "should I take the lump sum?" and treats any answer as advice | Real financial harm; regulatory exposure | Hard no-advice rule in system prompt; scripted handoff to a human advisor with contact details |
| **Silent failure** | Handbook index or DB is degraded; answers get thinner without saying so | Member can't tell "no data" from "wrong data" | Explicit degraded-mode messages (user-flow fail-safes); maintenance page rather than partial data |

## Highest-priority fix

**Hallucination**, closed via the citation-or-abstain rule. For a pension member, one confidently wrong explanation of their money undoes every correct table they've ever seen, and unlike opacity or control it can't be repaired by a UI affordance after the fact. It is also the cheapest gap to instrument: a code-based check that every numeric claim carries a resolvable payment ID or handbook section runs on 100% of traffic and is the first layer of the M6 eval stack.
