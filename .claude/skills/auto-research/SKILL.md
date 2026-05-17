---
name: auto-research
description: Autonomous wiki quality loop. Searches the internet for new information, scrapes with Playwright if needed, ingests findings, and improves weak pages. Runs unattended.
user_invocable: true
---

# Auto-Research: Wiki Quality Loop

Autonomous loop that finds weak spots in the wiki and improves them with internet research.

## Loop (repeat until 10 pages improved or 30 minutes elapsed)

1. **Find weakest page**
   - Score all pages by: word count, confidence level, last_verified date, number of sources
   - Pick the lowest-scoring page

2. **Research the topic**
   - Search the web for authoritative sources (official docs, GitHub, blog posts, papers)
   - For JS-rendered pages, use Playwright MCP to navigate and extract content
   - Cross-reference multiple sources to verify claims

3. **Save raw research**
   - Save scraped content to `raw/research/[topic]-[date].md`
   - Never modify existing raw files

4. **Improve the page**
   - Add missing information with source citations
   - Update stale claims
   - Raise confidence level if claims are now well-sourced
   - Update `last_verified` date
   - Add new `[[wikilinks]]` to related pages

5. **Create new pages if needed**
   - If research reveals topics not yet covered, create new pages
   - Update `wiki/index.md` with new entries

6. **Log the operation**
   - Append to `wiki/log.md`: what was researched, what changed, sources used

7. **Move to next page**

## Reporting

After the loop completes, report:
- Pages improved (with before/after confidence)
- New pages created
- Sources added to `raw/research/`
- Topics that need human input (contradictory sources, etc.)
