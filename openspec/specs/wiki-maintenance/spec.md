# Wiki Maintenance

## Purpose

TBD - Health checks, contradiction detection, stale claims, orphans, missing cross-references, and gap analysis.

## Requirements

### Requirement: /llmw-health-check command audits wiki
The system SHALL provide a `/llmw-health-check` command that audits the entire wiki for quality issues.

#### Scenario: User runs health check
- **WHEN** the user runs `/llmw-health-check`
- **THEN** the skill systematically checks for contradictions, stale claims, orphans, missing cross-references, and knowledge gaps

#### Scenario: Agent suggests health check
- **WHEN** the wiki has accumulated several ingestions (e.g., 5-10 new sources)
- **THEN** the agent may proactively suggest running `/llmw-health-check`

### Requirement: Contradiction detection
The system SHALL detect contradictory claims across wiki pages.

#### Scenario: Two pages make conflicting claims
- **WHEN** the agent finds pages that assert contradictory facts
- **THEN** the agent reports the contradiction, cites both pages and their sources, and offers to flag or resolve

#### Scenario: Newer source supersedes older claim
- **WHEN** a newer source provides updated information that contradicts an older claim
- **THEN** the agent suggests updating the older page and notes the superseded claim in the log

### Requirement: Stale claim identification
The system SHALL identify claims that may be outdated.

#### Scenario: Claim references old source
- **WHEN** a wiki claim is based on a source that is significantly older than more recent sources on the same topic
- **THEN** the agent flags the claim as potentially stale and suggests review

#### Scenario: Agent suggests source refresh
- **WHEN** stale claims are found
- **THEN** the agent suggests finding newer sources to verify or update the claims

### Requirement: Orphan page detection
The system SHALL identify wiki pages with no inbound links.

#### Scenario: Page has no backlinks
- **WHEN** a wiki page exists but no other wiki page links to it
- **THEN** the agent reports it as an orphan and suggests linking it from relevant pages or considering it for removal

### Requirement: Missing cross-reference detection
The system SHALL identify important concepts mentioned in pages that lack their own dedicated page.

#### Scenario: Concept mentioned but undocumented
- **WHEN** a concept or entity is referenced across multiple wiki pages but has no dedicated page
- **THEN** the agent reports it as a missing page and offers to create one from the accumulated references

#### Scenario: Agent creates missing pages
- **WHEN** the user approves creating missing pages during lint
- **THEN** the agent creates new pages synthesizing information from all pages that reference the concept

### Requirement: Knowledge gap analysis
The system SHALL identify knowledge gaps and suggest sources to fill them.

#### Scenario: Topic area has thin coverage
- **WHEN** a domain area has few pages or sparse content relative to related areas
- **THEN** the agent identifies the gap and suggests specific types of sources to seek out

#### Scenario: Agent suggests web searches
- **WHEN** a knowledge gap can likely be filled with publicly available information
- **THEN** the agent suggests specific web search queries or types of publications to look for

### Requirement: Lint results filed in wiki
The system SHALL record lint findings in the wiki for ongoing reference.

#### Scenario: Lint report saved
- **WHEN** a lint pass completes
- **THEN** the agent appends a dated entry to `wiki/log.md` summarizing findings and actions taken

#### Scenario: Lint issues tracked across sessions
- **WHEN** lint identifies issues that are not immediately resolved
- **THEN** the agent maintains a running list (in log or a dedicated page) so issues aren't forgotten between sessions
