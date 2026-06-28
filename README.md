# llmw — LLM Wiki

A reference implementation of [Karpathy's LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) pattern as skills. Build a persistent, compounding knowledge base where the LLM maintains all pages, cross-references, and consistency — you curate sources and ask questions.

## Commands

| Command | Description |
|---|---|
| `/llmw-init` | Bootstrap wiki structure — creates `raw/`, `wiki/`, and adds conventions to `AGENTS.md` |
| `/llmw-ingest <url>` | Fetch web content, cache in `raw/`, integrate into wiki pages, update index and log |
| `/llmw-query <question>` | Search the wiki, synthesize cited answers, offer to file results back |
| `/llmw-health-check` | Audit wiki for contradictions, stale claims, orphans, missing references, gaps |

## Directory Layout

```
.
├── raw/                   # Cached source content (immutable, LLM reads only)
├── wiki/                  # LLM-generated markdown pages
│   ├── index.md           # Catalog of all pages with one-line summaries
│   ├── log.md             # Append-only chronological log
│   └── overview.md        # High-level synthesis
├── .opencode/
│   ├── commands/          # /llmw-init, /llmw-ingest, /llmw-query, /llmw-health-check
│   └── skills/            # Skill implementations
└── AGENTS.md              # Full wiki conventions (page format, naming, cross-references)
```

## Quickstart

1. **Install skills** — copy `.opencode/commands/` and `.opencode/skills/` from this repo into your project
2. **Initialize** — run `/llmw-init` to create the wiki structure
3. **Ingest** — run `/llmw-ingest <url>` to add your first source

Then query with `/llmw-query <question>` and maintain with `/llmw-health-check`. Open `wiki/` as an Obsidian vault for graph view, backlinks, and browsing.

## Conventions

See generated AGENTS.md for the full specification: page naming (kebab-case), cross-reference syntax (`[[wikilinks]]`), frontmatter schema, and `index.md`/`log.md` formats.

## License

MIT — © 2026 Rubén Marín Rivarés
