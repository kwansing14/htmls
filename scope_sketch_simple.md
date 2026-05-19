## What the sketch shows

A **sourcing/procurement chat app** powered by Claude. Ingests emails (and later ERP/Excel), cleans them into a structured DB, and lets Sales/NPI/Production chat with the data — query status, compare quotes, draft vendor replies. Web search built in.

## Stack

- **Next.js** — chat UI + API routes
- **Postgres + pgvector** — structured data + embeddings for semantic search over emails/quotes
- **AI SDK** (Vercel) — chat streaming + tool use with Claude
- **Better Auth** — user login + Gmail/Outlook OAuth tokens
- **Vercel Cron** (or Inngest if you want retries) — periodic email polling
- **`xlsx` + `unpdf`** — parse quote attachments

Optional: **R2/S3** for raw attachment storage if you want to re-process later.

## How to build it

1. **Auth** — Better Auth with Google/Microsoft provider; store refresh tokens in Postgres.
2. **Ingest** — Cron job polls Gmail/Outlook API every N minutes; parses attachments; writes raw + extracted rows to Postgres.
3. **Clean** — Claude normalizes quotes, part numbers, pricing into structured tables. Embed key fields into pgvector.
4. **Chat** — AI SDK `streamText` with tools: `searchEmails`, `queryQuotes`, `compareVendors`, `webSearch`, `draftReply`.
5. **Vendor comms** — Agent drafts RFQs/follow-ups in chat; human approves before send.

## Build order

Start with **Gmail ingest → pgvector → chat with `searchEmails` tool**. That's the spike. Add quote extraction, then ERP, then send-on-approval.

Want me to scaffold the Next.js project, or go deeper on the ingestion pipeline or tool definitions?
