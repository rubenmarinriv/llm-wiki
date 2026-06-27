---
name: llmw-query
description: Query the LLM Wiki. Reads the index, finds relevant pages, synthesizes cited answers in multiple formats, and offers to file valuable answers back into the wiki.
license: MIT
compatibility: Requires OpenCode.
metadata:
  author: llmw
  version: "1.0"
---

Answer a question using the LLM Wiki as the knowledge base.

**Input**: `/llmw-query <question>`

**Steps**

1. **Validate wiki exists**

   Check that `wiki/index.md` exists. If not, tell the user to run `/llmw-init` first and stop.

2. **Read the index**

   Read `wiki/index.md` to discover what pages are available. Identify candidate pages whose summaries or category membership match the query topic.

3. **Read candidate pages**

   Read the most promising candidate pages. If the query is broad, read 3-5 pages. If specific, drill into 1-2.

4. **Synthesize answer**

   Compose a clear, cited answer. Every factual claim SHALL reference the source wiki page:
   - `[[page-name]]` for claims from a specific page
   - Chain citations where applicable: "[[summary-page]] (based on `raw/<source>.md`)"

   Format the answer as markdown with sections as appropriate.

5. **Offer multi-format output**

   Based on the question type, suggest the best format and offer alternatives:

   | Question type | Default format | Alternatives |
   |---|---|---|
   | Factual ("What is X?") | Markdown page | Slide deck (Marp) |
   | Comparison ("X vs Y") | Comparison table | Markdown page |
   | Timeline / history | Markdown page | Timeline chart |
   | "Present on..." | Slide deck (Marp) | Markdown page |
   | "Chart of..." | Python matplotlib | Table |

   The user may accept the default or choose an alternative. Generate accordingly.

6. **Offer to file answer**

   After presenting the answer, ask: "File this answer as a wiki page?"

   If the user says yes:
   - Create `wiki/<topic-slug>.md` with the answer content
   - Add appropriate YAML frontmatter (`tags`, `created`, `query: "<original question>"`)
   - Add entry to `wiki/index.md` under the appropriate category (Queries, or the relevant domain category)
   - Append a log entry:
     ```markdown
     ## [YYYY-MM-DD] query | <topic>
     - Filed answer: wiki/<topic-slug>.md
     - Question: <original question>
     ```

   If the user says no, the answer stays in chat only.

7. **Handle queries beyond wiki scope**

   If the wiki lacks sufficient information to answer:
   - Report what was found (if anything) and what's missing
   - Suggest specific URLs that might fill the gap
   - Offer: "I found partial information but not enough for a complete answer. Want me to ingest relevant sources first?"

**Output**

```
## <Question>

<!-- Synthesized answer with [[citations]] -->

---

*Answer based on N wiki pages. File this as a wiki page?*
```

**Guardrails**
- Always cite wiki pages, never present unattributed claims
- If wiki is uninitialized, stop and suggest `/llmw-init`
- If the answer is speculative, mark it clearly: "**Speculative**: ..."
- For multi-format outputs, confirm with user before generating non-markdown formats
- Answers should be accurate to what's in the wiki — don't hallucinate to fill gaps
