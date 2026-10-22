# Prior Authorization: Physician Approval & Payer Submission

`30 nodes` · **Stack:** n8n, Supabase (Postgres + encrypted PHI RPC), Availity API, OpenAI gpt-4o, Gmail

## Why I built this: the pain point
Prior authorizations delay patient care for days. Staff re-key the same clinical data into payer portals, and denials need appeal letters written from scratch. Automating this is risky, though: nothing should reach a payer without a physician's sign-off, and patient data must stay protected.

## Goal
Turn a physician's one-click Approve/Reject email into a payer submission, with patient identity data kept encrypted until the moment it's needed.

## How it works
- Approve and Reject webhook links; each link is validated (exists, not expired, not already used)
- Patient name and DOB are decrypted through a Supabase RPC only at submission time
- Availity OAuth → eligibility inquiry → prior-auth submission
- Routing on the payer's answer: approved → care team; denied → AI-drafted appeal letter to the physician; other → logged as pending

## Guardrails and design decisions
- **Human in the loop:** nothing is submitted without the physician's explicit approval; Reject goes to manual staff review
- **Stops before submission** when coverage isn't active
- **Secrets in n8n Variables** (`$vars.SUPABASE_SERVICE_KEY`, `$vars.AVAILITY_CLIENT_SECRET`), never in the JSON
- The appeal letter is a draft only; the physician reviews and submits it

## Known limitations / next steps
- Workflow A (clinical extraction + approval email) is not included
- Approval links are not signed yet
- Built as a prototype; HIPAA/vendor compliance review is out of scope

## Setup
1. In n8n: **Workflows → Import from File** and select the JSON file(s) in this folder.
2. Open each node marked with a red credential warning and connect your own credential.
3. Replace placeholders such as `YOUR_GOOGLE_SHEET_ID` and `https://YOUR-N8N-HOST`.
4. Add any variables from the repo's `.env.example` under **Settings → Variables**.
5. Test with sample data, then activate.

> The exported JSON contains **no API keys, credential IDs, sheet IDs or captured execution data.** They were removed with `scripts/sanitize.py`.
