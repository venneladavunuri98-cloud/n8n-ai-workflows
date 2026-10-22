# RAG Website Support Chatbot

`32 nodes` · **Stack:** n8n, OpenAI (gpt-4.1-mini + embeddings), Supabase pgvector, Postgres

## Why I built this: the pain point
Website visitors ask the same questions about services, pricing and process, and a generic chatbot either invents answers or goes stale as soon as the site changes.

## Goal
Answer visitor questions only from the company's own website content, cite the source, and keep the knowledge base fresh automatically.

## How it works
- **Chat pipeline:** AI Agent (gpt-4.1-mini) with session memory and a Supabase pgvector retriever
- **Nightly ingestion (02:00):** fetches 10 pages, cleans HTML, hashes content, and re-embeds only changed pages (800-char chunks, 150 overlap); stale vectors are deleted first
- Crawl errors are logged to a `crawl_errors` table with 3 retries

## Guardrails and design decisions
- **Answers only from retrieved content,** with a source citation; declines legal, medical and financial advice and binding price commitments
- Prompt-injection and confidentiality rules in the system prompt
- **Change detection by content hash** avoids re-embedding unchanged pages (cost control)
- Parameterized SQL on all Postgres nodes

## Known limitations / next steps
- Fixed URL list (no sitemap discovery); no JS-rendered pages
- No human hand-off or lead capture yet

## Setup
1. In n8n: **Workflows → Import from File** and select the JSON file(s) in this folder.
2. Open each node marked with a red credential warning and connect your own credential.
3. Replace placeholders such as `YOUR_GOOGLE_SHEET_ID` and `https://YOUR-N8N-HOST`.
4. Add any variables from the repo's `.env.example` under **Settings → Variables**.
5. Test with sample data, then activate.

> The exported JSON contains **no API keys, credential IDs, sheet IDs or captured execution data.** They were removed with `scripts/sanitize.py`.
