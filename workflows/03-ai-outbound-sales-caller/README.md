# AI Outbound Sales Caller

`3 workflows · 80 nodes` · **Stack:** n8n, OmniDimension, OpenAI (gpt-4.1-mini, gpt-4o-mini), Google Sheets, Google Calendar, Gmail/SMTP

## Why I built this: the pain point
SDRs spend most of their day researching leads and dialling numbers that don't pick up. Generic AI callers make claims the company can't back up, call outside legal hours, and ignore do-not-call requests.

## Goal
Turn a new lead row into a researched, grounded phone pitch, dial it inside legal calling hours, and act on the outcome. Every claim about the company must come from an approved knowledge base.

## How it works
- **Lead intake & enrichment:** crawls the prospect's site (with SSRF protection), profiles pain points, picks the top 3 knowledge-base rows and writes a pitch with citation checks
- **Call dispatcher (every 2 min):** the single place that places calls; enforces DNC, a 09:00–18:00 local window (time zone from area code or country), max 4 attempts, one call in flight
- **Live on-call RAG:** a secured webhook the voice agent can query mid-call, capped at 2 lookups per call
- **Post-call processing:** classifies the outcome, schedules retries, adds DNC numbers, and books a 30-min meeting only when strict evidence checks pass

## Guardrails and design decisions
- **Grounded honesty:** the agent may only state facts found in the knowledge sheet; it never quotes prices that aren't there
- **Verbatim evidence:** any action triggered by a transcript (booking, DNC) must be backed by an exact quote
- Do-not-call list and calling-hour rules are enforced in code, not in the prompt
- Error-alert email to the team for any failed execution

## Known limitations / next steps
- Google Sheets acts as the CRM (no HubSpot/Salesforce integration)
- English only; no voicemail drops
- Calling-regulation coverage is limited to DNC and calling hours

## Setup
1. In n8n: **Workflows → Import from File** and select the JSON file(s) in this folder.
2. Open each node marked with a red credential warning and connect your own credential.
3. Replace placeholders such as `YOUR_GOOGLE_SHEET_ID` and `https://YOUR-N8N-HOST`.
4. Add any variables from the repo's `.env.example` under **Settings → Variables**.
5. Test with sample data, then activate.

> The exported JSON contains **no API keys, credential IDs, sheet IDs or captured execution data.** They were removed with `scripts/sanitize.py`.
