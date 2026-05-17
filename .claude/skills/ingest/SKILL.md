---
name: ingest
description: Ingest a new source document into the wiki. Reads a raw source file, creates summary and concept pages, and updates the index.
user_invocable: true
---

# Ingest Source Document

Given a file path in `raw/`:

1. Read the source document
2. Discuss key takeaways with the user
3. Create `wiki/sources/[source-name].md` summary
4. Update or create concept/entity/workflow pages as needed
5. Update `wiki/index.md` with new entries
6. Append to `wiki/log.md`
