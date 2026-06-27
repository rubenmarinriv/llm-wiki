## 1. /llmw-init — Bootstrap skill + command

- [x] 1.1 Create `.opencode/commands/llmw-init.md` (user-facing command: accepts no args, invokes `llmw-init` skill)
- [x] 1.2 Create `.opencode/skills/llmw-init/SKILL.md` with logic: create `raw/` and `wiki/` dirs, create `wiki/index.md`, `wiki/log.md`, `wiki/overview.md` stubs, add wiki conventions section to AGENTS.md, install the other 3 skill + command files
- [x] 1.3 Idempotency: detect existing structure, only add missing pieces, never overwrite wiki content

## 2. /llmw-ingest — URL ingestion skill + command

- [x] 2.1 Create `.opencode/commands/llmw-ingest.md` (accepts `<url>` argument, invokes `llmw-ingest` skill)
- [x] 2.2 Create `.opencode/skills/llmw-ingest/SKILL.md` with logic: fetch URL content → cache raw text in `raw/<slug>.md` → read and discuss takeaways → create summary page in `wiki/` → update entity/concept pages → update `wiki/index.md` → append `wiki/log.md`
- [x] 2.3 Handle multiple URLs (sequential processing, summary across all)
- [x] 2.4 Entity/concept page logic: create if new, merge if existing, flag contradictions, note source URL

## 3. /llmw-query — Knowledge query skill + command

- [x] 3.1 Create `.opencode/commands/llmw-query.md` (accepts `<question>` argument, invokes `llmw-query` skill)
- [x] 3.2 Create `.opencode/skills/llmw-query/SKILL.md` with logic: read `wiki/index.md` → identify candidate pages → read and synthesize answer with citations → offer to file answer as new wiki page → update index/log if filed
- [x] 3.3 Multi-format output support (markdown page, comparison table, Marp slide deck)
- [x] 3.4 Handle queries beyond wiki scope (report gaps, suggest URLs to ingest)

## 4. /llmw-health-check — Maintenance skill + command

- [x] 4.1 Create `.opencode/commands/llmw-health-check.md` (accepts no args, invokes `llmw-health-check` skill)
- [x] 4.2 Create `.opencode/skills/llmw-health-check/SKILL.md` with logic: systematic scan for contradictions, stale claims, orphans, missing cross-references, knowledge gaps → report findings → append log entry
- [x] 4.3 Contradiction detection: compare claims across pages, flag conflicts, cite sources
- [x] 4.4 Orphan detection: pages with no inbound links
- [x] 4.5 Gap analysis: thin coverage areas, suggest URLs to ingest

## 5. AGENTS.md conventions

- [x] 5.1 Add "## LLM Wiki" section to AGENTS.md with: directory layout, page naming (kebab-case), cross-reference conventions (`[[link]]`), frontmatter schema, index.md format, log.md format
- [x] 5.2 This section is written by `/llmw-init` on first run; kept up to date as conventions evolve

## 6. End-to-end verification

- [x] 6.1 Run `/llmw-init` in a clean test project, verify all 8 files created (4 commands + 4 skills) and directories exist
- [x] 6.2 Run `/llmw-ingest <karpathy-essay-url>`, verify summary page created, index/log updated, entity pages populated
- [x] 6.3 Run `/llmw-query "What is the LLM Wiki pattern?"`, verify cited answer from ingested content
- [x] 6.4 Run `/llmw-health-check`, verify report includes findings (even if none — "wiki is healthy")
