---
name: llmw-init
description: Initialize the LLM Wiki in a project — creates directory structure and adds wiki conventions to AGENTS.md. Works for new or existing projects. Skill files must already be present (via package or copy).
license: MIT
metadata:
  author: llmw
  version: "1.0"
---

Initialize the LLM Wiki in a project. Creates the wiki directory structure and adds wiki conventions to AGENTS.md. This command only bootstraps per-project scaffolding, not the skills themselves.

**Input**: None (run as `/llmw-init`).

**Steps**

1. **Check for existing structure**

   Check if `raw/`, `wiki/`, `wiki/index.md`, `wiki/log.md`, `wiki/overview.md` already exist.

   Report what's already present and what's missing.

2. **Create directories** (skip if exist)

   ```
   raw/
   wiki/
   ```

3. **Create wiki stub files** (skip if exist)

   **`wiki/index.md`** — empty catalog with category headers:
   ```markdown
   # Wiki Index

   ## Entities

   ## Concepts

   ## Sources

   ## Comparisons

   ## Queries
   ```

   **`wiki/log.md`** — empty log with header and init entry:
   ```markdown
   # Wiki Log

   ## [YYYY-MM-DD] init | LLM Wiki initialized
   - Created wiki structure
   ```

   **`wiki/overview.md`** — placeholder overview:
   ```markdown
   # Overview

   This wiki is just getting started. Use `/llmw-ingest <url>` to add your first source.
   ```

4. **Add or update AGENTS.md conventions**

   Read the project's `AGENTS.md` (create if it doesn't exist).

   Check if a `## LLM Wiki` section exists. If not, append the following section:

   ```markdown
   ## LLM Wiki

   This project uses an LLM-maintained wiki. The LLM owns `wiki/`; humans curate `raw/`.

   ### Commands
   - `/llmw-init` — Initialize wiki structure (safe to re-run)
   - `/llmw-ingest <url>` — Ingest web content into the wiki
   - `/llmw-query <question>` — Query the wiki with citations
   - `/llmw-health-check` — Audit wiki for issues

   ### Directory layout
   ```
   raw/    # Cached source content (immutable, LLM reads only)
   wiki/   # LLM-generated markdown pages
     index.md     # Catalog of all pages with one-line summaries
     log.md       # Append-only chronological log
     overview.md  # High-level synthesis
   ```

   ### Page conventions
   - **Filenames**: kebab-case (`reinforcement-learning.md`)
   - **Cross-references**: `[[page-name]]` wiki-link syntax
   - **Frontmatter** (optional):
     ```yaml
     ---
     tags: [tag1, tag2]
     created: YYYY-MM-DD
     updated: YYYY-MM-DD
     source_count: N
     ---
     ```

   ### Index format (`wiki/index.md`)
   Categorized sections with one entry per page:
   ```markdown
   ## Entities
   - [[page-name]] — One-line summary
   ```

   ### Log format (`wiki/log.md`)
   Append-only, each entry starts with `## [YYYY-MM-DD] type | description`:
   ```markdown
   ## [2026-01-15] ingest | Article: Some Title
   - Created: page-a.md, page-b.md
   - Updated: page-c.md
   - Pages affected: 3
   ```
   Parseable: `grep "^## \[" wiki/log.md | tail -5`
   ```

   If the section already exists, leave it unchanged.

6. **Report summary**

   List what was created and what was skipped (already present):
   ```
   ## LLM Wiki initialized

   Created:
   - raw/
   - wiki/ (index.md, log.md, overview.md)
   - AGENTS.md updated with wiki conventions

   Already present (skipped):
   - (list any)

   Next: run `/llmw-ingest <url>` to add your first source.
   ```

**Guardrails**
- Never overwrite existing files or wiki content
- Always report what was created vs skipped
- This command does NOT create or update skill/command files — those are a distribution concern
- If AGENTS.md already has a `## LLM Wiki` section, leave it unchanged
