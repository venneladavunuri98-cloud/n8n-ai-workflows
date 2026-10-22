# Insurance Renewal Reminders

`24 nodes` · **Stack:** n8n, Google Sheets, Gmail/SMTP

## Why I built this: the pain point
Policies lapse because nobody reminds the customer in time, and manual reminder lists either miss people or email them twice.

## Goal
Send 90-, 60- and 30-day renewal reminders automatically, exactly once each, and never to lapsed or cancelled policies.

## How it works
- Daily 8:00 AM IST schedule reads every policy
- Calculates the 90/60/30-day dates and writes them and a clean status label back to the sheet
- Three parallel checks send each reminder only on its exact date; the 30-day email is marked URGENT

## Guardrails and design decisions
- **No duplicate emails:** each reminder is sent only if today is the reminder date, the policy is eligible and the `email_sent_X` flag isn't already "Sent"
- **Blocked statuses** (lapsed, cancelled, withdrawn, expired, void, terminated) never receive reminders
- UTC-safe date maths

## Known limitations / next steps
- A reminder is missed if the workflow is down on that exact date (no catch-up yet)

## Setup
1. In n8n: **Workflows → Import from File** and select the JSON file(s) in this folder.
2. Open each node marked with a red credential warning and connect your own credential.
3. Replace placeholders such as `YOUR_GOOGLE_SHEET_ID` and `https://YOUR-N8N-HOST`.
4. Add any variables from the repo's `.env.example` under **Settings → Variables**.
5. Test with sample data, then activate.

> The exported JSON contains **no API keys, credential IDs, sheet IDs or captured execution data.** They were removed with `scripts/sanitize.py`.
