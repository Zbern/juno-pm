# AI PRD · Juno

> Module 3 · RAG / AI PRD. The AI product requirements doc with retrieval requirements, built with the **M3 · AI PRD Builder** (RAG design from the **M3 · RAG Architecture Decider**). Paste the tool's markdown over this file.

## Problem & user

_The user problem and who has it._

_____

## Solution overview

_What Juno does, at a glance._

_____

## Retrieval requirements (RAG)

- **Sources:** _what Juno retrieves from._
- **Chunking / indexing:** _strategy + why._
- **Grounding rule:** _e.g. no answer without a cited source._
- **Freshness:** _how current the data must be._

## Requirements

| # | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| 1 | _…_ | Must | _…_ |
| 2 | _…_ | Should | _…_ |

## Out of scope
RocketShip, Q3 2026 Strategy One-Pager

NORTH STAR
Become the fastest, most reliable analytics platform for mid-market data teams who currently rely on Excel + Salesforce.

THREE STRATEGIC PILLARS

1. Reliability First
   The platform must work. Every export, every report, every load. We are losing enterprise deals (Pearson Co, Acme) because of P0 reliability bugs - CSV export crashes, 403 permission errors, queue overflows on large reports. ZERO new feature work ships if the legacy reporting API is redlining.

2. Enterprise Compliance
   We win or lose on SAML/SSO, audit logs, and role-based permissions. Acme deal worth $200k ARR is dead if we don't ship Okta SSO by Oct 1. This is not optional.

3. Speed-to-Insight
   Mid-market analysts choose us over Salesforce because we feel fast. Anything that makes the dashboard slower (heavy AI summarization on top of fragile DBs, 30+ second exports) is a step backwards.

WHAT WE ARE NOT DOING THIS QUARTER
- Aesthetic refreshes (dark mode, color palette tweaks, "make it pop" UI work)
- Competitor-mimicking AI features without clear user evidence
- TikTok integration or other social plumbing
- Dashboard summarization that requires DB sharding work

DECISION RULES
- If a request reduces reliability OR slows export speed -> P3 or notRecommended
- If a request unblocks an enterprise deal with stated $ARR -> P0
- If a request comes from "exec opinion" without user evidence -> notRecommended
- If a request fixes a workflow blocker (CSV crash, permission error) -> P0/P1
_____
