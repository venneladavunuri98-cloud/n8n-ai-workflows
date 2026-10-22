# Hospital Voice Appointment Assistant

`51 nodes · 5 tool webhooks` · **Stack:** Vapi, n8n, Google Calendar, Google Sheets

## Why I built this: the pain point
Hospital front desks lose hours to calls that only book, cancel or move a 30-minute slot. Patients wait on hold, and double bookings happen when calendars and spreadsheets drift apart.

## Goal
Give callers a voice receptionist that can list doctors, check a slot, and book, cancel or reschedule, with each doctor's Google Calendar as the single source of truth.

## How it works
- Vapi assistant (GPT-4.1, Deepgram) with 5 tools, each backed by one n8n webhook
- Doctor master data and appointment log in Google Sheets; one Google Calendar per doctor
- Speech-friendly result strings, including clear not-found and failure messages

## Guardrails and design decisions
- **Calendar is the source of truth:** availability is checked live before every booking
- Fixed 30-minute IST slots keep scheduling logic simple and predictable
- Cancel and reschedule match on patient name plus date, time or doctor to avoid touching the wrong booking

## Known limitations / next steps
- Webhook authentication not yet enabled (next step: Vapi secret + Header Auth on each webhook)
- No patient identity verification or SMS confirmations
- Does not suggest alternative free slots yet

## Setup
1. In n8n: **Workflows → Import from File** and select the JSON file(s) in this folder.
2. Open each node marked with a red credential warning and connect your own credential.
3. Replace placeholders such as `YOUR_GOOGLE_SHEET_ID` and `https://YOUR-N8N-HOST`.
4. Add any variables from the repo's `.env.example` under **Settings → Variables**.
5. Test with sample data, then activate.

> The exported JSON contains **no API keys, credential IDs, sheet IDs or captured execution data.** They were removed with `scripts/sanitize.py`.
