# Insurance Quote Generation

`26 nodes` · **Stack:** n8n, Google Gemini, Google Sheets, Gmail

## Why I built this: the pain point
Agents compare carrier plans by hand for every enquiry, which is slow, inconsistent between agents, and easy to get wrong when a customer needs several coverages within a budget.

## Goal
Collect requirements through a form, validate them with AI, score every carrier plan against the customer's budget and coverages, and email the best options, with every quote tracked in a sheet.

## How it works
- Form trigger: name, age, budget, required coverages, email
- AI validation of the submission (Gemini)
- Carrier plans read from Google Sheets (premium, bodily injury limits, collision and comprehensive coverage and deductibles)
- Rule-based scoring and ranking → recommendation email → tracking sheet update

## Guardrails and design decisions
- **Scoring is deterministic code, not the LLM:** the AI only validates input, so the same request always gets the same ranking
- Every quote is logged for follow-up and audit

## Known limitations / next steps
- Plan data is maintained manually in the sheet
- No live carrier API rating

## Setup
1. In n8n: **Workflows → Import from File** and select the JSON file(s) in this folder.
2. Open each node marked with a red credential warning and connect your own credential.
3. Replace placeholders such as `YOUR_GOOGLE_SHEET_ID` and `https://YOUR-N8N-HOST`.
4. Add any variables from the repo's `.env.example` under **Settings → Variables**.
5. Test with sample data, then activate.

> The exported JSON contains **no API keys, credential IDs, sheet IDs or captured execution data.** They were removed with `scripts/sanitize.py`.
