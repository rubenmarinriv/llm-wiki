# Wiki Structure

## Purpose

TBD - Wiki directory layout, naming, cross-references, index, log, and overview conventions.

## Requirements

### Requirement: /llmw-init command bootstraps wiki
The system SHALL provide a `/llmw-init` command that bootstraps the wiki directory structure and conventions into a project.

#### Scenario: New project initialized
- **WHEN** a user runs `/llmw-init` in a project without wiki structure
- **THEN** the skill creates `raw/` and `wiki/` directories, creates `wiki/index.md`, `wiki/log.md`, and `wiki/overview.md` stubs, and adds wiki conventions to `AGENTS.md`

#### Scenario: Existing project re-initialized
- **WHEN** a user runs `/llmw-init` in a project that already has wiki structure
- **THEN** the skill detects existing directories and stub files, only adds missing pieces, and does not overwrite existing wiki content

#### Scenario: Skills must be pre-installed
- **WHEN** a user runs `/llmw-init`
- **THEN** the skill assumes all 4 LLM Wiki commands and skills already exist under `.opencode/`; it does not create or modify skill files

#### Scenario: AGENTS.md updated on init
- **WHEN** `/llmw-init` runs
- **THEN** the skill adds or updates a "## LLM Wiki" section in `AGENTS.md` with directory layout, page naming conventions, cross-reference syntax, and frontmatter expectations

### Requirement: Wiki directory layout
The system SHALL use a consistent directory structure with `raw/` for immutable cached sources and `wiki/` for LLM-generated pages.

#### Scenario: Directory layout after init
- **WHEN** `/llmw-init` completes
- **THEN** the project has `raw/` and `wiki/` directories at the project root

### Requirement: Index file for navigation
The system SHALL maintain a `wiki/index.md` file that catalogs every page in the wiki with a link and one-line summary.

#### Scenario: Index is created on first ingest
- **WHEN** the first source is ingested
- **THEN** the agent creates `wiki/index.md` with categorized sections (entities, concepts, sources, etc.)

#### Scenario: Index is updated after every ingest
- **WHEN** a source ingestion creates or modifies wiki pages
- **THEN** the agent updates `wiki/index.md` to reflect new, changed, or removed pages

#### Scenario: Agent uses index for query discovery
- **WHEN** the user asks a question against the wiki
- **THEN** the agent reads `wiki/index.md` first to find relevant pages before drilling into them

### Requirement: Chronological log file
The system SHALL maintain an append-only `wiki/log.md` file recording every ingest, query, and lint operation with timestamps.

#### Scenario: Log entry after ingestion
- **WHEN** a source is ingested
- **THEN** the agent appends a dated entry to `wiki/log.md` describing the source, what was created/updated, and page count affected

#### Scenario: Log is parseable with grep
- **WHEN** each log entry starts with `## [YYYY-MM-DD]` prefix
- **THEN** running `grep "^## \[" wiki/log.md | tail -5` returns the last 5 entries

### Requirement: Page naming conventions
The system SHALL use kebab-case filenames for all wiki pages.

#### Scenario: Creating a new wiki page
- **WHEN** the agent creates a page for a concept, entity, or summary
- **THEN** the filename uses kebab-case (e.g., `reinforcement-learning.md`, `gpt-architecture.md`)

#### Scenario: Special pages
- **WHEN** creating `index.md`, `log.md`, or overview pages
- **THEN** these use lowercase names without kebab-case prefix (`index.md`, `log.md`, `overview.md`)

### Requirement: Cross-reference conventions
The system SHALL use standard markdown wiki-links for cross-references between pages.

#### Scenario: Linking between wiki pages
- **WHEN** the agent references another wiki page in a page body
- **THEN** it uses `[[page-name]]` wiki-link syntax for Obsidian compatibility

#### Scenario: Backlinks are implicit
- **WHEN** a page links to another page
- **THEN** the target page gains a backlink visible in Obsidian's graph view and backlinks pane

### Requirement: YAML frontmatter on wiki pages
The system MAY include YAML frontmatter on wiki pages with optional metadata fields.

#### Scenario: Standard frontmatter fields
- **WHEN** a wiki page is created
- **THEN** it MAY include frontmatter with `tags`, `created`, `updated`, and `source_count` fields

#### Scenario: Frontmatter is optional
- **WHEN** a team does not use Dataview or frontmatter-based tooling
- **THEN** wiki pages function correctly without YAML frontmatter

### Requirement: Auto-generated overview page
The system SHALL maintain a `wiki/overview.md` page that provides a high-level synthesis of all ingested knowledge.

#### Scenario: Overview updated after ingestion
- **WHEN** a new source is ingested that changes the understanding of the domain
- **THEN** the agent updates `wiki/overview.md` to reflect the evolving synthesis

#### Scenario: Overview serves as entry point
- **WHEN** a user or agent explores the wiki
- **THEN** `wiki/overview.md` provides the entry-point summary of what the wiki contains
