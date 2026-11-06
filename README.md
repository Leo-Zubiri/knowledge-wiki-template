# Knowledge Repository Template — LLM Wiki & Second Brain

A clone-and-go template for a **domain knowledge repository** that is, at the same time:

- a human **Second Brain** you can edit and explore in **Obsidian**, and
- an **LLM Wiki** that AI agents (Claude Code, Codex, …) read, search, and contribute to.

Everything is **plain Markdown + YAML + Git**. No application, service, database, or index is required. Obsidian
and MCP are *optional enhancements* — they improve the experience but never own or gate the knowledge.

> **Central principle:** the repository must stay fully usable by humans and agents through ordinary files and Git.
> If you deleted every tool, a clone would still be a readable, version-controlled knowledge base.

## Who reads what
- **Humans:** start here, then open the repo as an Obsidian vault for editing, graph, and search
  (see [`meta/obsidian.md`](meta/obsidian.md)).
- **Agents:** read [`AGENTS.md`](AGENTS.md) — the canonical, filesystem-first entry point. Agents do **not** need
  Obsidian or MCP to work.

## The three layers
1. **Knowledge** (source of truth): Markdown + YAML frontmatter + `[[wikilinks]]` + Git.
2. **Agent access** (headless): an agent's own filesystem tools — `ripgrep` search, file reads, link traversal,
   and `git` for changes. Works with **Obsidian closed and MCP absent**.
3. **Obsidian integration** (human + optional MCP): editing, Graph, Bases, and an optional
   [Local REST API (with MCP)](meta/mcp-setup.md) surface for agents **while Obsidian is open**.

## Layout
```
.
├── README.md            ← you are here (human orientation)
├── AGENTS.md            ← canonical agent entry point (filesystem-first)
├── inbox/               ← quick capture: unprocessed, mutable, never cited
├── sources/             ← preserved originals (conventionally immutable) + provenance
├── notes/               ← the wiki: synthesized concept / pattern / decision / reference notes
└── meta/                ← everything *about* the repo
    ├── methodology/     ← CODE · PARA · Zettelkasten · LLM-wiki · the lifecycle
    ├── workflows/       ← how to capture, preserve, synthesize, retrieve, and govern
    ├── templates/       ← note templates (one per content type)
    ├── obsidian.md      ← Obsidian configuration guidance
    ├── mcp-setup.md     ← optional Local REST API (with MCP) setup
    └── security.md      ← security & operational notes
```
An **optional** PARA task layer (`projects/`, `areas/`) is described in
[`meta/methodology/methodologies.md`](meta/methodology/methodologies.md); create it only if your domain tracks
active work.

## Create a new domain repository from this template
1. **Clone / copy** this template into a new repo named `<domain>-knowledge` (e.g. `laravel-knowledge`).
2. **Set your domain:** pick the `domain:` value you will use in frontmatter (e.g. `laravel`).
3. **Configure (optional):** open it in Obsidian; optionally set up the
   [Local REST API (with MCP)](meta/mcp-setup.md) plugin for agent access while Obsidian is open.
4. **Add sources:** drop material into [`inbox/`](inbox/) and preserve originals into [`sources/`](sources/).
5. **Ingest:** follow the [workflows](meta/workflows/) — capture → preserve → synthesize.
6. **Extend:** add domain-specific frontmatter fields and note templates **additively** (never change the core).
7. **Maintain & version:** update existing notes instead of duplicating; let Git record the history.

See [`meta/methodology/metadata.md`](meta/methodology/metadata.md) for the frontmatter conventions and
[`meta/workflows/`](meta/workflows/) for the step-by-step processes.

## What this template does *not* include (by design)
No custom CLI, database, search index, retrieval engine, embeddings/vector store, or custom MCP server. These are
deferred until a measured need justifies them (see [`AGENTS.md`](AGENTS.md) → "When the filesystem is not enough").
