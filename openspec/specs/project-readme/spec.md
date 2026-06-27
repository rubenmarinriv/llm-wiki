# project-readme

## Purpose

TBD — The `README.md` at the repo root serves as the project's landing page, introducing the project, documenting commands, and guiding new adopters.

## Requirements

### Requirement: README introduces the project
The system SHALL provide a `README.md` at the repo root that introduces the project to first-time visitors.

#### Scenario: Visitor reads README
- **WHEN** someone visits the repository on GitHub or opens it locally
- **THEN** the README clearly states what the project is and why it exists

### Requirement: README documents all commands
The system SHALL document all 4 LLM Wiki commands with usage examples in the README.

#### Scenario: User learns command syntax from README
- **WHEN** a user reads the README
- **THEN** they find `/llmw-init`, `/llmw-ingest <url>`, `/llmw-query <question>`, and `/llmw-health-check` with brief descriptions and example usage

### Requirement: README includes quickstart
The system SHALL include a 3-step quickstart section for new adopters.

#### Scenario: New team adopts the project
- **WHEN** a new team wants to use the LLM Wiki
- **THEN** the README provides steps: install skills → init → ingest first source

### Requirement: README shows directory layout
The system SHALL include a visual directory layout showing the key parts of the project.

#### Scenario: User understands project structure
- **WHEN** a user opens the README
- **THEN** they see a tree showing `wiki/`, `raw/`, `.opencode/`, and what each contains

### Requirement: README links to AGENTS.md
The system SHALL link to `AGENTS.md` for detailed conventions rather than duplicating them.

#### Scenario: User wants deeper documentation
- **WHEN** a user reads the README and wants more detail
- **THEN** they are directed to `AGENTS.md` for full conventions, page formats, and index/log specifications
