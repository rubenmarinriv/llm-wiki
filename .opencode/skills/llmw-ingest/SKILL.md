---
name: llmw-ingest
description: Ingest web content from URLs into the LLM Wiki. Fetches page content, caches raw text, creates summary pages, updates entity/concept pages, and maintains index.md and log.md.
license: MIT
metadata:
  author: llmw
  version: "1.0"
---

Ingest content from one or more URLs into the LLM Wiki.

**Input**: `/llmw-ingest <url1> [url2 ...]`

**Steps**

1. **Validate wiki exists**

   Check that `wiki/index.md`, `wiki/log.md` exist. If not, tell the user to run `/llmw-init` first and stop.

2. **Fetch each URL**

   For each URL provided:
   - Use the **WebFetch tool** to retrieve the page content as markdown
   - If a URL fails to fetch, report the error and continue with remaining URLs

3. **Cache raw content**

   For each URL, derive a slug from the page title or URL path (kebab-case). Save the fetched content to `raw/<slug>.md` with this header:
   ```markdown
   # <Page Title>
   - **Source**: <url>
   - **Fetched**: YYYY-MM-DD
   ```

4. **Read and discuss takeaways** (interactive, per URL)

   For each URL:
   - Read the fetched content from `raw/<slug>.md`
   - Present 3-5 key takeaways to the user
   - Ask: "What should I emphasize? Any specific focus areas?" (allow user to guide emphasis)
   - If user provides no guidance, proceed with balanced coverage

5. **Create summary page**

   For each URL, create `wiki/<slug>.md`:
   ```markdown
   ---
   tags: [extracted-from-content]
   created: YYYY-MM-DD
   source: <url>
   ---

   # <Page Title>

   ## Key Points
   - Point 1
   - Point 2

   ## Summary
   <!-- 2-4 paragraphs synthesizing the content -->

   ## Notable Quotes
   > Relevant quote 1
   > Relevant quote 2

   ## Related
   - [[entity-x]] — connection
   - [[concept-y]] — connection
   ```

6. **Update entity and concept pages**

   Identify entities and concepts mentioned in the source. For each:

   - **If no page exists**: Create `wiki/<entity-name>.md` with available info and a `source: <url>` reference
   - **If page exists**: Read it, merge new information, note the new source. If new info contradicts existing claims, flag it:
     ```markdown
     > **Contradiction**: [New claim from <url>] conflicts with [prior claim from <other-source>]. Review needed.
     ```
   - Add cross-references (`[[other-page]]`) to connect related pages

7. **Update `wiki/index.md`**

   For each created or substantially updated page:
   - If page is new: add entry under the appropriate category section (Entities, Concepts, Sources, etc.)
   - If page was updated: update its one-line summary if the content changed meaningfully

8. **Append `wiki/log.md`**

   Add a dated entry:
   ```markdown
   ## [YYYY-MM-DD] ingest | <Page Title>
   - Source: <url>
   - Created: page-a.md, page-b.md
   - Updated: page-c.md, page-d.md
   - Pages affected: N
   ```

9. **Report summary**

   After all URLs are processed, display:
   ```
   ## Ingestion complete

   Processed N URL(s):
   1. <title> (<url>) — created X pages, updated Y pages
   2. ...

   Total: N pages created, M pages updated
   ```

**Multi-URL mode**

When multiple URLs are provided:
- Process sequentially (one at a time)
- After all are processed, optionally create a cross-source synthesis if the sources are related
- Each URL gets its own summary page and log entry

**Guardrails**
- Never modify files in `raw/` after caching (read-only)
- Always present takeaways before making edits — let the user guide emphasis
- Flag contradictions, don't silently overwrite
- Keep summary pages focused (~1-3 screens of content)
- If wiki is uninitialized, stop and suggest `/llmw-init`
- Use kebab-case for all filenames derived from titles/URLs
