# Knowledge Query

## Purpose

TBD - Answering questions against the wiki with citations, multi-format output, and filing answers back.

## Requirements

### Requirement: /llmw-query command answers against wiki
The system SHALL provide a `/llmw-query <question>` command that answers questions using the wiki as its knowledge base.

#### Scenario: Agent reads index before answering
- **WHEN** the user runs `/llmw-query "What is X?"`
- **THEN** the skill reads `wiki/index.md`, identifies candidate pages, then reads those pages to synthesize an answer

#### Scenario: Index points to relevant pages
- **WHEN** the index contains entries matching the query topic
- **THEN** the agent drills into those pages and synthesizes an answer with citations

### Requirement: Synthesis with citations
The system SHALL provide answers with citations to wiki pages and, where applicable, original sources.

#### Scenario: Answer includes page citations
- **WHEN** the agent synthesizes an answer from multiple wiki pages
- **THEN** each claim in the answer references the source wiki page (e.g., `[[page-name]]`)

#### Scenario: Answer traces back to original source
- **WHEN** a wiki page itself references a raw source
- **THEN** the answer may include a chain: claim → wiki page → raw source

### Requirement: Multi-format answer generation
The system SHALL support generating answers in multiple output formats based on the question type.

#### Scenario: Standard markdown answer
- **WHEN** the user asks a factual question
- **THEN** the agent returns a markdown page with citations

#### Scenario: Comparison table answer
- **WHEN** the user asks to compare multiple entities or concepts
- **THEN** the agent generates a markdown comparison table

#### Scenario: Slide deck answer
- **WHEN** the user requests a presentation format
- **THEN** the agent generates a Marp-compatible markdown slide deck

### Requirement: Filing answers back into wiki
The system SHALL offer to save valuable query answers as new wiki pages.

#### Scenario: Agent offers to file answer
- **WHEN** the agent generates an answer that synthesizes new insights
- **THEN** the agent asks the user if they want to file the answer as a new wiki page

#### Scenario: Answer filed as wiki page
- **WHEN** the user confirms filing an answer
- **THEN** the agent saves the answer as a new `wiki/<topic>.md` page and updates index and log accordingly

#### Scenario: User declines filing
- **WHEN** the user declines to file an answer
- **THEN** the answer remains in chat history only and is not persisted to the wiki

### Requirement: Query results compound knowledge
The system SHALL treat valuable queries as contributions to the knowledge base, not throwaway interactions.

#### Scenario: Comparison becomes permanent reference
- **WHEN** the user asks for a comparison between two concepts and files the result
- **THEN** future queries can reference and build upon the filed comparison page

#### Scenario: Index reflects filed queries
- **WHEN** a query answer is filed as a wiki page
- **THEN** it appears in `wiki/index.md` alongside ingested source pages

### Requirement: Handling queries beyond wiki scope
The system SHALL identify when a query cannot be answered from the wiki and suggest actions.

#### Scenario: Wiki lacks information
- **WHEN** the user asks a question the wiki cannot answer
- **THEN** the agent reports what it found (or didn't find) and suggests relevant sources to add or web searches to perform

#### Scenario: Agent suggests source gap
- **WHEN** the wiki has partial but insufficient information
- **THEN** the agent identifies the specific knowledge gap and suggests what kind of source would fill it
