---
name: ingest-api
description: Fetch API documentation from a URL, save as raw source, extract endpoints, and create structured wiki pages. Handles JS-rendered docs via Playwright.
user_invocable: true
---

# Ingest API Documentation

Given a URL to API documentation:

1. **Fetch the docs**
   - Try `WebFetch` first for static pages
   - If content is empty/incomplete (JS-rendered), use Playwright MCP to navigate and extract
   - Save raw content to `raw/apis/[service-name].md`

2. **Extract structured info**
   - Base URL and authentication method
   - All endpoints: method, path, parameters, request/response schemas
   - Rate limits, pagination patterns, error codes
   - WebSocket/streaming capabilities if any
   - Code examples from the docs

3. **Create wiki pages**
   - `wiki/sources/api-[service-name].md` — Full endpoint reference table
   - Update or create relevant workflow pages showing practical usage
   - Add automation examples using `curl`, Python `requests`, or Claude Code's Bash tool

4. **Update index and log**
   - Add new pages to `wiki/index.md`
   - Append operation to `wiki/log.md`

5. **Report what was created**
   - List all pages created/updated
   - Highlight any endpoints that seem particularly useful
   - Flag any gaps (undocumented endpoints, missing auth info)

## Usage

```
/ingest-api https://docs.example.com/api
```

Pass the URL to the API documentation as the argument.
