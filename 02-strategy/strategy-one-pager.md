AI Strategy One-Pager · Juno
1. Problem & Workflow
Members who want to check or understand a pension payment currently sign in, navigate four screens, download a PDF statement, and then call support when a figure doesn't match what they expected. The workflow is: notice something → find the statement → try to interpret it → contact support. Juno collapses this to: sign in → see history → ask why.
2. Target Metrics
Metric	Baseline (today)	V1 target
Median time from sign-in to viewing a specific payment	~4 min, 4 screens	< 60 s, 1 screen
Task completion rate ("found the payment I was looking for", exit survey)	61%	≥ 85%
Support contacts tagged "payment question" per 1,000 active members / month	38	≤ 20
Chat answers with a valid payment-ID or handbook citation (M6 eval)	n/a	≥ 98%
Member-filed dispute rate (reports ÷ history views, daily)	unknown	tracked; > 5% triggers escalation
Baseline figures are placeholders to replace with real analytics.
3. Autonomy Level
Members: Copilot. Juno explains and offers actions (export, filter, file a report); the member decides. No autonomous changes to any record.
Internal: Agent with human-in-the-loop. A triage agent aggregates member reports and posts digests; a PM reviews anything above the 5% dispute threshold before action is taken.
4. Data & Model Approach
Modular RAG over three sources: live Pension Payments DB (member-scoped), Employee Pension Handbook (hybrid semantic + keyword index), HR eligibility API. Hosted frontier model via private endpoint, zero data retention, no fine-tuning in V1. Grounding rule: every figure cites a payment ID or handbook section, or Juno abstains.
5. Risks & Mitigations
Risk	Mitigation
Wrong member's data exposed	member_id bound at session; retrieval filtered server-side, not by prompt; step-up identity verification before any personal data
Hallucinated explanation of a payment	mandatory citation; abstain below 70% confidence; "report this payment" always one click away
PII leakage to model vendor	private endpoint, zero retention, PII never in logs, contractual DPA
Members treat explanation as financial advice	explicit no-advice rule + link to human advisor
Data-quality problem surfaces as a wave of disputes	triage agent escalates at 5%, on-call PM can trigger maintenance mode at 15%
6. V1 Scope
In: payment history (last 5 years), payment explanations grounded in records + handbook, CSV/PDF export, report missing/incorrect payment, internal triage digest.
Out: pension applications, benefit projections or "what-if" calculators, any change to payment or personal records, tax/financial advice, channels other than web.
