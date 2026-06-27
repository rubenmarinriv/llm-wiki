## Context

Karpathy's LLM Wiki proposes a persistent, LLM-maintained knowledge base built from raw sources. The LLM agent acts as the wiki "programmer" — ingesting sources, updating pages, maintaining cross-references, and answering queries. The human curates sources, guides analysis, and asks questions.

This design implements the pattern as 4 OpenCode skills (with matching commands) that any team can drop into their project. The "code" is agent behavior, and the deliverables are `.opencode/skills/` and `.opencode/commands/` files. The repo itself serves as a reference implementation that any team can adopt.

## Goals / Non-Goals

**Goals:**
- Define a turnkey wiki structure an LLM agent can create and maintain in any project
- Deliver ingestion, query, and maintenance workflows as OpenCode skills + commands (`/llmw-init`, `/llmw-ingest`, `/llmw-query`, `/llmw-health-check`)
- Keep zero application dependencies — pure markdown + agent instructions
- Make adoption simple: run `/llmw-init`, start ingesting URLs via `/llmw-ingest`
- Co-evolvable: teams customize skills and AGENTS.md conventions as their domain needs grow

**Non-Goals:**
- Not building a custom search engine or embedding pipeline (optional, use qmd or similar)
- Not building Obsidian plugins or UI (Obsidian is a compatible viewer, not a dependency)
- Not implementing RAG or vector databases as core infrastructure
- Not handling non-text sources (images handled as referenced files, not processed)
- Not a multi-user collaborative editing system

## Decisions

**Decision 1: Wiki lives as file tree, not database**
The wiki is a directory of markdown files. This is version-controllable, diffable, and accessible to any markdown viewer (Obsidian, VS Code, GitHub). No database, no API server, no runtime dependency.

**Alternatives considered:** SQLite-backed structured store — rejected because it adds a dependency and loses git-native history and plaintext portability.

**Decision 2: index.md as primary navigation, not full-text search**
At moderate scale (~hundreds of pages, ~100 sources), an LLM can read a well-maintained index file to find relevant pages. This avoids embedding infrastructure until scale demands it.

**Alternatives considered:** BM25 search from day one — rejected as premature; add when index.md becomes unwieldy.

**Decision 3: OpenCode skills and commands as the delivery mechanism**
Each capability is delivered as a paired OpenCode skill + command under `.opencode/`. The command triggers the skill, the skill contains the detailed workflow instructions. AGENTS.md holds the wiki structure conventions (naming, formats, cross-references) that all skills reference.

**Alternatives considered:** Single monolithic AGENTS.md section — rejected because skills encapsulate workflows better, allow independent evolution, and follow OpenCode's native extension model.

**Decision 4: Four skills mapped 1:1 to capabilities**
Each capability becomes its own skill/command pair:
- `wiki-structure` → `/llmw-init` (bootstraps wiki + installs skills, works for new or existing projects)
- `source-ingestion` → `/llmw-ingest` (ingests from URL)
- `knowledge-query` → `/llmw-query` (queries wiki, files answers back)
- `wiki-maintenance` → `/llmw-health-check` (lint and health checks)

**Decision 5: URL-based ingestion, not local file crawl**
`/llmw-ingest` accepts a URL. The skill fetches the page content, caches the raw text in `raw/`, and then processes it into the wiki. This is simpler for adoption (no file management needed) and works with any web content. Local file ingestion is a future extension.

**Alternatives considered:** Local file watching with directory crawling — rejected as more complex to set up and less universally applicable.

## Risks / Trade-offs

- **[Scale ceiling on index.md]** At very large scale, reading the full index per query becomes expensive. → Add search engine (qmd or custom) when needed; the spec accommodates this.
- **[Agent context window limits]** Very large wiki pages may overflow agent context. → Wiki pages should be kept focused (~1-3 screens); split long pages into sub-pages.
- **[Human-in-the-loop overhead]** Ingestion is interactive by default (discuss, review, guide). → Batch ingestion mode is supported for less supervision; user chooses their cadence.
- **[Git noise from frequent wiki updates]** Every ingest touches multiple files. → Encourage meaningful commit messages; wiki changes are low-noise markdown diffs.

## Open Questions

- Should `raw/` live inside `wiki/` or at repo root? (Default: repo root for clear separation)
- Should index.md be a flat list or a tree with YAML frontmatter for tooling? (Default: marked sections with flat lists; frontmatter optional for Dataview users)
- How does batch ingestion handle inter-source dependencies? (TBD: defer to experience; start with sequential processing)
- Should `/llmw-init` be idempotent (safe to run on already-initialized projects)? (Yes — detect existing structure, only add missing pieces)
