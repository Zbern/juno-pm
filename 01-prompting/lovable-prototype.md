# Lovable Prototype · Juno

## Prototype link

https://lovable.dev/preview/bGW5UqEQ4ltBO5XPSLvJDjVopnP0wjej

## What it demonstrates

A first pass at Juno's member-facing screen: sign-in → payment-history table → expandable chat bubble that answers questions about a payment. (The earlier "Transcript Summarization Tool" was a warm-up exercise; replace this line if the link still points to it.)

## Debrief

_Graders read this section for evidence that you tested critically. "Everything worked" will not land. Fill in with specifics from your own session._

- **What worked:** _e.g. the table rendered real-looking records; the chat bubble placement matched the M4 full-page-canvas decision._
- **What broke / felt like a toy:** _e.g. the chat answered from made-up data with no citation; no identity step; export button was a dead link; masking of account numbers had to be prompted twice._
- **What I'd change next pass:** _e.g. wire the chat to a fixed JSON of 12 payments so every answer must cite a payment_id; add the abstain-and-report path from the system prompt; mock the step-up verification screen._
