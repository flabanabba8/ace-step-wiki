# ACE-Step 1.5 Knowledge Wiki

You are an ACE-Step 1.5 assistant. Help users understand and use ACE-Step effectively by consulting and maintaining this wiki.

**Always check wiki/ before answering.** Read `wiki/index.md` first, then relevant pages.

## Structure

- `raw/` — Immutable source documents. Never modify.
- `wiki/` — Maintained knowledge pages
  - `wiki/index.md` — Master catalog. Update on every change.
  - `wiki/log.md` — Append-only operation log
  - `wiki/concepts/` — Core concepts (architecture, models, tasks)
  - `wiki/entities/` — Tools, UIs, integrations
  - `wiki/workflows/` — Step-by-step guides
  - `wiki/how-to/` — Task-oriented guides
  - `wiki/sources/` — Source document summaries

## Page Format

Every page needs YAML frontmatter:
```yaml
---
title: Page Title
type: concept | entity | workflow | how-to | source-summary
tldr: "Under 50 chars"
related:
  - "[[related-page]]"
created: YYYY-MM-DD
updated: YYYY-MM-DD
confidence: high | medium | low
---
```

- Filenames: kebab-case
- Cross-references: `[[wikilinks]]`
- Every claim should be traceable to documentation or testing
- Mark contradictions with `> [!contradiction]` callouts

## Workflows

### Answering Questions (Anti-Evaporation)

1. Read `wiki/index.md` to identify relevant pages
2. Read those pages and synthesize an answer
3. If the wiki doesn't cover the topic, check `raw/` sources
4. If still insufficient, research the internet
5. **If the answer is novel and valuable, file it as a new wiki page**

### Ingest (Adding Sources)

1. Read the source document in `raw/`
2. Create `wiki/sources/[source-name].md` summary
3. Update or create concept/entity/workflow pages
4. Update `wiki/index.md` with new entries
5. Append to `wiki/log.md`

### Ingest API Docs (`/ingest-api`)

1. Fetch the API documentation URL (use Playwright for JS-rendered pages)
2. Save the raw content to `raw/apis/[service-name].md`
3. Extract: base URL, authentication method, all endpoints with methods/params/responses
4. Create `wiki/sources/api-[service-name].md` with structured endpoint reference
5. Create or update relevant workflow pages showing how to use the API
6. Update `wiki/index.md` and `wiki/log.md`

### Auto-Research (`/auto-research`)

1. Identify topics the wiki covers poorly (thin pages, low confidence, missing areas)
2. Search the web for authoritative sources on each topic
3. For JS-rendered pages, use Playwright to scrape content
4. Save raw scraped content to `raw/research/[topic]-[date].md`
5. Ingest findings into existing wiki pages or create new ones
6. Verify claims against multiple sources before raising confidence
7. Log all operations to `wiki/log.md`
8. After 10 pages or 30 minutes, report what was improved
