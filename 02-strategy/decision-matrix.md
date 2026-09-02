# AI Solution Decision Matrix · Juno

## The decision

Should the pension payment-history experience be a static, filterable web page, or include an LLM + RAG copilot that explains payments in natural language? And if the latter, do we build, buy, fine-tune, or partner for the AI layer?

**Why now:** Member satisfaction on "understanding my payments" has fallen for three consecutive quarters, and "where is / why is my payment X" is the top support-contact reason. The data (payments DB, handbook, HR eligibility) already exists and is structured; the gap is explanation, not information.

## Options scored

Scale 1–5, **higher is better** on every column (so Cost 5 = cheap, Risk 5 = low risk). Weights reflect that this is regulated PII: control and risk outrank speed.

| Option | Cost (w=1) | Speed (w=1) | Control (w=2) | Moat (w=1) | Risk (w=2) | Weighted score |
|---|---|---|---|---|---|---|
| **Build** RAG + orchestration in-house; call a hosted model via API with zero data retention | 3 | 3 | 5 | 3 | 4 | **4.0** |
| **Buy** an off-the-shelf "chat with your data" SaaS | 4 | 5 | 2 | 1 | 2 | 2.6 |
| **Fine-tune** a model on pension Q&A | 2 | 2 | 4 | 4 | 3 | 3.1 |
| **Partner** with our pension-admin platform vendor's AI add-on | 4 | 4 | 2 | 1 | 3 | 2.7 |

Weighted score = Σ(score × weight) ÷ 7.

## Recommendation

**Build the retrieval and data layer; buy the model.** The moat is our data plumbing and grounding rules, not the language model. Fine-tuning solves a problem we don't have (the handbook changes and payments are live, so retrieval beats memorised weights). Buying or partnering would mean member PII leaves our control boundary, which compliance will not approve.

Concretely for V1: a modular RAG pipeline over payments DB + handbook, a hosted frontier model behind a private endpoint with no training on our data, and a static-first UI where the chat is optional. Revisit fine-tuning only if eval data (M6) shows persistent tone or domain-vocabulary failures that prompting cannot fix.
