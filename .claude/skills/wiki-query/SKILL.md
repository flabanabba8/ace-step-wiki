---
name: wiki-query
description: Answer a question about ACE-Step by consulting the wiki. Navigates via index, reads relevant pages, synthesizes an answer with citations. Files novel findings back as wiki pages.
user_invocable: true
---

# Wiki Query

1. Read `wiki/index.md` to identify relevant pages
2. Read those pages and synthesize an answer
3. Cite sources using `[[wikilinks]]`
4. If the wiki doesn't cover the topic, check `raw/` sources
5. If still insufficient, search the web
6. **If the answer is novel and valuable, file it as a new wiki page** (anti-evaporation)
