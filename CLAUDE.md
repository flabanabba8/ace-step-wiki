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
