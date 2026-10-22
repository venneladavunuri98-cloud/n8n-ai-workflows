# AI Automation Workflows (n8n)

Production-style automations I built as an AI Automation Engineer: voice agents, RAG, healthcare and insurance workflows. Each one is designed around the same rule:

> **The AI drafts, retrieves or talks. Code verifies. A human (or a hard rule) makes the decisions that matter.**

## ⭐ Flagship projects (full standalone repos)
| Project | Repo |
|---|---|
| AI Outbound Sales Caller: grounded pitches, DNC and calling-hour rules in code, evidence-checked booking | [ai-outbound-sales-caller](https://github.com/venneladavunuri98-cloud/ai-outbound-sales-caller) |
| Restaurant Voice Ordering Agent: 6 tool webhooks, 12-table Postgres schema, audit log, human handoff | [restaurant-voice-ordering-agent](https://github.com/venneladavunuri98-cloud/restaurant-voice-ordering-agent) |
| AI Lead Finder: Apollo company + people search, buying-signal detection, LLM analysis, deterministic 0–100 scoring | [ai-lead-finder](https://github.com/venneladavunuri98-cloud/ai-lead-finder) |
| Prior Authorization: physician approval, late PHI decryption, Availity submission, AI appeal drafts | [prior-authorization-approval](https://github.com/venneladavunuri98-cloud/prior-authorization-approval) |
| RFP Intelligence Platform: LangGraph, citation verification, 165 tests | [RFP-Intelligence-Platform](https://github.com/venneladavunuri98-cloud/RFP-Intelligence-Platform) |

## All workflows

| Workflow | Pain point it solves | Stack |
|---|---|---|
| [Restaurant Voice Ordering Agent](workflows/01-restaurant-voice-ordering-agent/) | Restaurants miss orders when the phone line is busy at peak hours, and staff spend that time reading out the menu, prices and allergens instead of cooking. | OmniDimension (voice + LLM), n8n, PostgreSQL / Supabase |
| [Hospital Voice Appointment Assistant](workflows/02-hospital-voice-appointment-assistant/) | Hospital front desks lose hours to calls that only book, cancel or move a 30-minute slot. | Vapi, n8n, Google Calendar, Google Sheets |
| [AI Outbound Sales Caller](workflows/03-ai-outbound-sales-caller/) | SDRs spend most of their day researching leads and dialling numbers that don't pick up. | n8n, OmniDimension, OpenAI (gpt-4.1-mini, gpt-4o-mini), Google Sheets, Google Calendar, Gmail/SMTP |
| [Prior Authorization: Physician Approval & Payer Submission](workflows/04-prior-authorization-approval/) | Prior authorizations delay patient care for days. | n8n, Supabase (Postgres + encrypted PHI RPC), Availity API, OpenAI gpt-4o, Gmail |
| [RAG Website Support Chatbot](workflows/05-rag-website-support-chatbot/) | Website visitors ask the same questions about services, pricing and process, and a generic chatbot either invents answers or goes stale as soon as the site changes. | n8n, OpenAI (gpt-4.1-mini + embeddings), Supabase pgvector, Postgres |
| [Insurance Quote Generation](workflows/06-insurance-quote-generation/) | Agents compare carrier plans by hand for every enquiry, which is slow, inconsistent between agents, and easy to get wrong when a customer needs several coverages within a budget. | n8n, Google Gemini, Google Sheets, Gmail |
| [Insurance Renewal Reminders](workflows/07-insurance-renewal-reminders/) | Policies lapse because nobody reminds the customer in time, and manual reminder lists either miss people or email them twice. | n8n, Google Sheets, Gmail/SMTP |
| [Flight Price Drop Tracker](workflows/08-flight-price-drop-tracker/) | Fares change many times a day, and checking a route by hand until it hits your budget is tedious. | n8n, SerpApi (Google Flights), Google Sheets, Gmail |

## Patterns you'll see across these workflows
- **Grounding:** agents answer only from live tools, a knowledge base or retrieved documents, never from memory
- **Human in the loop:** orders, payer submissions and meetings are confirmed by a person or by strict evidence checks
- **Deterministic logic in code:** scoring, eligibility, calling hours, DNC and duplicate prevention are code, not prompts
- **Auditability:** tool calls, decisions and errors are logged
- **Security hygiene:** credentials live in n8n's credential store or Variables; every export is sanitized before it's committed

## Repo layout
```
workflows/<name>/workflow.json   sanitized n8n export (import into n8n)
workflows/<name>/README.md       pain point, design, guardrails, setup
scripts/sanitize.py              strips secrets/PII from raw exports
scripts/check_secrets.py         pre-commit check that blocks secrets/PII
.env.example                     variables each workflow expects (no values)
```

## Adding a new workflow safely
```bash
python scripts/sanitize.py raw/my_export.json workflows/09-new/workflow.json
python scripts/check_secrets.py
```

---
Built by **Vennela Davunuri** · [Portfolio](https://portfolio-vennela3.vercel.app) · [LinkedIn](https://www.linkedin.com/in/venneladavunuri/)
