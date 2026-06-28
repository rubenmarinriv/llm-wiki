---
name: llmw-health-check
description: Audit the LLM Wiki for quality issues. Detects contradictions, stale claims, orphan pages, missing cross-references, and knowledge gaps. Reports findings and logs results.
license: MIT
metadata:
  author: llmw
  version: "1.0"
---

Run a systematic health check on the LLM Wiki.

**Input**: None (run as `/llmw-health-check`).

**Steps**

1. **Validate wiki exists**

   Check that `wiki/index.md` exists. If not, tell the user to run `/llmw-init` first and stop.

2. **Read the index**

   Read `wiki/index.md` to discover all wiki pages. Build a map of what exists.

3. **Read all wiki pages**

   Read every page listed in the index (excluding `index.md` and `log.md`). For large wikis, sample or batch-read pages.

4. **Check for contradictions**

   Compare claims across pages that discuss the same entities or concepts:

   - If two pages make conflicting factual claims, flag them:
     ```
     Contradiction: [[page-a]] claims X, but [[page-b]] claims Y
     Sources: <url-a>, <url-b>
     ```
   - If a newer source contradicts an older claim, note the superseding:
     ```
     Stale claim in [[page-a]]: claims X (source from 2023)
     Newer source [[page-b]] (2026) suggests Y
     ```

5. **Check for stale claims**

   For each wiki page, examine source dates in the frontmatter or page references:
   - Flag claims where the source is significantly older than newer sources on the same topic
   - Suggest pages that may need refresh

6. **Check for orphan pages**

   For each wiki page (excluding `index.md`, `log.md`, `overview.md`):
   - Search all other wiki pages for `[[page-name]]` references to this page
   - If no inbound links found, report: "Orphan: [[page-name]] — no other pages link to it"
   - Suggest linking it from relevant pages or considering removal
   - Exception: query-filed pages and overview page may have few inbound links — flag as low priority

7. **Check for missing cross-references**

   Scan all pages for concepts or entities mentioned but lacking their own page:
   - Look for capitalized terms, unfamiliar names, or domain-specific jargon mentioned across multiple pages
   - Report: "Missing page: 'Concept X' referenced in [[page-a]], [[page-b]], but has no dedicated page"
   - Offer to create stub pages from accumulated references

8. **Analyze knowledge gaps**

   Compare coverage across domain areas:
   - Identify topics with thin coverage (few pages, sparse content) relative to related areas
   - For each gap, suggest specific types of sources or URLs to seek out

9. **Report findings**

   Present a structured report:
   ```
   ## Wiki Health Check — YYYY-MM-DD

   **Pages reviewed**: N
   **Issues found**: M

   ### Contradictions
   - (list or "None found")

   ### Stale Claims
   - (list or "None found")

   ### Orphan Pages
   - (list or "None found")

   ### Missing Cross-References
   - (list or "None found")

   ### Knowledge Gaps
   - (list or "None found")

   ### Suggested Actions
   1. ...
   2. ...
   ```

10. **Append `wiki/log.md`**

    ```markdown
    ## [YYYY-MM-DD] health-check
    - Pages reviewed: N
    - Issues found: M
    - Contradictions: X, Stale: Y, Orphans: Z, Missing refs: W, Gaps: V
    ```

**Guardrails**
- If wiki is uninitialized, stop and suggest `/llmw-init`
- Be constructive, not alarmist — most wikis have some issues
- Don't auto-fix issues; report and suggest actions
- If the wiki is large, prioritize high-signal pages (overview, entity hubs)
- Contradiction detection is best-effort — false positives are possible, mark with confidence level
