# Restaurant Voice Ordering Agent

`66 nodes · 6 tool webhooks` · **Stack:** OmniDimension (voice + LLM), n8n, PostgreSQL / Supabase

## Why I built this: the pain point
Restaurants miss orders when the phone line is busy at peak hours, and staff spend that time reading out the menu, prices and allergens instead of cooking. A voice bot that guesses a price or misses an allergy note is worse than no bot at all.

## Goal
Let a voice agent ("Riya") answer the ordering line, answer menu questions from live data only, build the cart, capture every special instruction word for word, and then hand the confirmed order to a human. The agent never places the order itself.

## How it works
- Six tools (getMenu, getHours, getLoyalty, manageCart, getSpecials, callHuman), each an n8n webhook with the same shape: Webhook → Parse → Resolve branch → Query → Format → Respond → Log
- PostgreSQL `voiceai` schema (12 tables): branches, menu, customers, promotions, calls, orders, order items, special instructions, tool-call audit log
- Branch is resolved from the dialled number, so one backend serves many outlets

## Guardrails and design decisions
- **Grounded answers only:** the system prompt forbids answering from memory; every fact comes from a tool result
- **Human closes every order:** confirmed orders go to staff through `callHuman` with a structured summary
- **Full audit trail:** every tool call (success or failure) is logged with an idempotent key, without slowing the caller's response
- **Parameterized SQL** and `retryOnFail` on live lookups; header auth on every webhook
- Immediate escalation for anger, manager requests, health/safety issues or repeated tool failures

## Known limitations / next steps
- No live POS integration yet; orders are finalized by a human
- No payment handling

## Setup
1. In n8n: **Workflows → Import from File** and select the JSON file(s) in this folder.
2. Open each node marked with a red credential warning and connect your own credential.
3. Replace placeholders such as `YOUR_GOOGLE_SHEET_ID` and `https://YOUR-N8N-HOST`.
4. Add any variables from the repo's `.env.example` under **Settings → Variables**.
5. Test with sample data, then activate.

> The exported JSON contains **no API keys, credential IDs, sheet IDs or captured execution data.** They were removed with `scripts/sanitize.py`.
