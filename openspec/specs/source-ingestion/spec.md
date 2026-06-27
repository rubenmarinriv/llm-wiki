# Source Ingestion

## Purpose

TBD - Ingesting web content, creating summary pages, updating entities, and caching raw sources.

## Requirements

### Requirement: /llmw-ingest command processes URLs
The system SHALL provide a `/llmw-ingest <url>` command that fetches content from a URL and integrates it into the wiki.

#### Scenario: User ingests a URL
- **WHEN** the user runs `/llmw-ingest https://example.com/article`
- **THEN** the skill fetches the page content, caches the raw text in `raw/`, extracts key information, and integrates it into the wiki

#### Scenario: Agent reads source and discusses with user
- **WHEN** the agent finishes fetching and reading a source
- **THEN** the agent presents key takeaways to the user and asks what to emphasize before making edits

#### Scenario: Multiple URLs ingested sequentially
- **WHEN** the user provides multiple URLs
- **THEN** the agent processes them sequentially, noting key findings across all sources in a summary

### Requirement: Summary page creation
The system SHALL create a summary page for each ingested source.

#### Scenario: Summary page created in wiki
- **WHEN** a source is ingested
- **THEN** the agent creates `wiki/<source-name>.md` with key points, main arguments, and relevant quotes

#### Scenario: Summary page includes source reference
- **WHEN** a summary page is created
- **THEN** it includes a link to the original URL and a reference to the cached copy in `raw/`

### Requirement: Entity and concept page updates
The system SHALL update existing entity and concept pages when new sources add or change information.

#### Scenario: New entity discovered
- **WHEN** a source mentions an entity that has no wiki page
- **THEN** the agent creates a new entity page with the available information

#### Scenario: Existing entity page updated
- **WHEN** a source provides new information about an existing entity
- **THEN** the agent updates the entity page, noting the new source and any contradictions with prior claims

#### Scenario: Contradiction flagging
- **WHEN** a new source directly contradicts an existing wiki claim
- **THEN** the agent flags the contradiction on the relevant page and notes both sources

### Requirement: Index and log updates during ingestion
The system SHALL update `wiki/index.md` and `wiki/log.md` as part of every ingestion.

#### Scenario: Index entries added
- **WHEN** new pages are created during ingestion
- **THEN** the agent adds entries to the appropriate category section in `wiki/index.md`

#### Scenario: Index entries modified
- **WHEN** existing pages are substantially updated
- **THEN** the agent updates the corresponding one-line summary in `wiki/index.md`

#### Scenario: Log entry appended
- **WHEN** ingestion completes
- **THEN** the agent appends a dated entry to `wiki/log.md` listing the source, pages created, pages updated, and total page count affected

### Requirement: Multi-page impact per source
The system SHALL update all affected pages when a single source is ingested, which may touch 10-15 wiki pages.

#### Scenario: Source touches multiple entity pages
- **WHEN** a source discusses multiple entities and concepts
- **THEN** the agent updates all relevant pages in a single ingestion pass

#### Scenario: Agent reports scope of changes
- **WHEN** ingestion completes
- **THEN** the agent reports to the user how many pages were created and how many were updated

### Requirement: Raw source caching
The system SHALL cache fetched web content in `raw/` as immutable reference copies.

#### Scenario: URL content cached on ingest
- **WHEN** `/llmw-ingest` fetches a URL
- **THEN** the raw text is saved to `raw/<slug>.md` (or `.txt`) as an immutable reference copy

#### Scenario: Agent reads cache but never modifies
- **WHEN** the agent needs the original source text
- **THEN** it reads from `raw/` but writes only to `wiki/`
