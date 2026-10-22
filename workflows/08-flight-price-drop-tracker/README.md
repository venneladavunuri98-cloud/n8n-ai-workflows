# Flight Price Drop Tracker

`11 nodes` · **Stack:** n8n, SerpApi (Google Flights), Google Sheets, Gmail

## Why I built this: the pain point
Fares change many times a day, and checking a route by hand until it hits your budget is tedious.

## Goal
Let anyone submit a route and a target price, then check live Google Flights prices every 4 hours and email them when the price drops below target.

## How it works
- Form → watchlist row in Google Sheets
- Every 4 hours: read the watchlist, query SerpApi (Google Flights) per row, take the lowest price across best and other flights
- Email alert when the price is under target; write the last checked price and time back to the sheet

## Guardrails and design decisions
- Processes rows one by one to respect API rate limits
- SerpApi key is stored as an n8n credential (query auth), not in the workflow

## Known limitations / next steps
- Repeat alerts while the price stays low
- No price history or currency conversion (USD only)

## Setup
1. In n8n: **Workflows → Import from File** and select the JSON file(s) in this folder.
2. Open each node marked with a red credential warning and connect your own credential.
3. Replace placeholders such as `YOUR_GOOGLE_SHEET_ID` and `https://YOUR-N8N-HOST`.
4. Add any variables from the repo's `.env.example` under **Settings → Variables**.
5. Test with sample data, then activate.

> The exported JSON contains **no API keys, credential IDs, sheet IDs or captured execution data.** They were removed with `scripts/sanitize.py`.
