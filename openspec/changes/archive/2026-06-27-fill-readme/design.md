## Context

The repo currently has no README.md. The project is a reference implementation of the LLM Wiki pattern as OpenCode skills, plus the wiki content itself. Visitors need to understand what this is at a glance.

## Goals / Non-Goals

**Goals:**
- Create a single `README.md` that explains the project to first-time visitors
- Document all 4 commands with usage examples
- Show directory layout
- Provide adoption instructions

**Non-Goals:**
- Not duplicating AGENTS.md or wiki content
- Not creating a tutorial or full documentation site
- Not adding badges, CI status, or contributor guidelines

## Decisions

**Decision: Keep README short, point to AGENTS.md for details**
The README is the entry point. AGENTS.md already has the full conventions reference. README links there for deeper info rather than repeating content.

**Decision: Include quickstart section**
A 3-step getting-started flow: install skills → init → ingest. This is the most important thing for new users.

## Risks / Trade-offs

None — this is a documentation-only change with no code impact.
