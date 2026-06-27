## Why

Karpathy's LLM Wiki is a powerful pattern for building persistent, compounding knowledge bases using LLMs — but it currently lives as an abstract idea document. There's no turnkey implementation that lets a team drop it into a new or existing project and immediately start using it. By implementing this as an OpenSpec-managed set of agent instructions, skills, and conventions, teams get a structured, co-evolvable LLM Wiki that works with their existing LLM agent workflow on day one.

## What Changes

- Create 4 OpenCode skills under `.opencode/skills/` and corresponding commands under `.opencode/commands/`:
  - `/llmw-init` — bootstraps wiki structure and skills, works for new or existing projects
  - `/llmw-ingest` — ingests content from a URL, extracts key info, integrates into wiki
  - `/llmw-query` — searches the wiki, synthesizes answers with citations, offers to file results back
  - `/llmw-health-check` — periodic lint/health-check for contradictions, stale claims, orphans, gaps
- Define the wiki directory structure: `wiki/` for generated pages, `raw/` for immutable cached sources, `wiki/index.md` for navigation, `wiki/log.md` for chronology
- AGENTS.md conventions for wiki page formats, naming, cross-references, and frontmatter

## Capabilities

### New Capabilities

- `wiki-structure`: Directory layout, page naming conventions, frontmatter schema, index.md and log.md format, cross-reference conventions. Delivered via `/llmw-init`.
- `source-ingestion`: Workflow for ingesting content from a URL — fetching, reading, discussion, summary page creation, entity page updates, index/log updates. Delivered via `/llmw-ingest`.
- `knowledge-query`: Workflow for querying the wiki — page discovery via index, synthesis with citations, filing valuable answers back into the wiki. Delivered via `/llmw-query`.
- `wiki-maintenance`: Health-check workflow — contradiction detection, stale claim identification, orphan page detection, gap analysis. Delivered via `/llmw-health-check`.

### Modified Capabilities

None (this is a new, standalone capability set).

## Impact

- New directories: `wiki/`, `raw/` at project root (created by `/llmw-init`)
- New files: 4 command files under `.opencode/commands/`, 4 skill files under `.opencode/skills/`
- Updated file: `AGENTS.md` gets wiki conventions section (added by `/llmw-init`)
- No application code — this is an LLM-agent workflow delivered as OpenCode commands and skills
- Optional tools: search engine (qmd or equivalent), Obsidian integration, Marp for presentations
