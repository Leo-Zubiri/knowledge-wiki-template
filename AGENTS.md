# AGENTS.md — how AI agents work in this knowledge repository

This is the **canonical entry point for agents**. Read it first. It is **filesystem-first**: everything here works
with **Obsidian closed** and **MCP unavailable**, using only your ordinary file, search, and `git` tools. Obsidian/MCP
are optional accelerators (see [`meta/mcp-setup.md`](meta/mcp-setup.md)), never a requirement.

Whether a given tool auto-discovers this file varies by product — see
[`meta/interoperability.md`](meta/interoperability.md) for the verified support matrix and the two optional adapters
(`CLAUDE.md` = `@AGENTS.md`, `.gemini/settings.json`). We do **not** claim universal compatibility; the guarantee is
Level 1 — any agent that can read files can use this repo.

> **Prime directive:** the knowledge lives in plain Markdown + Git. Treat the files as the source of truth. Never
> assume a running service. An **agent observation is never verified knowledge** until a human verifies it.

## The repository in one minute
- `inbox/` — quick, unprocessed capture. Mutable. **Never cited** as knowledge.
- `sources/` — preserved originals + provenance. **Conventionally immutable** (don't rewrite history here).
- `notes/` — the wiki: synthesized, own-words notes with citations and links. This is what you read to answer.
- `meta/` — the rules: [methodology](meta/methodology/) (incl. [note-types](meta/methodology/note-types.md),
  [relationships](meta/methodology/relationships.md), [metadata](meta/methodology/metadata.md),
  [lifecycle](meta/methodology/lifecycle.md)), [workflows](meta/workflows/), [templates](meta/templates/),
  [interoperability](meta/interoperability.md), and integration/security docs.

Each note carries YAML frontmatter with three **independent** dimensions — see
[`meta/methodology/metadata.md`](meta/methodology/metadata.md):
- **type** (what it is): `source · concept · pattern · decision · reference · anti-pattern · guide · index`
  (pick with [note-types.md](meta/methodology/note-types.md)).
- **status** (trust) — **scoped by type**: claim-bearing notes use `unreviewed → proposed → reviewed → verified →
  deprecated`; `decision` uses `proposed → accepted → superseded`/`rejected`; `source` and `index` carry **no status**
  (see [lifecycle.md](meta/methodology/lifecycle.md)).
- **location** (where it lives): the folder (`inbox/ | sources/ | notes/`) — not a field.

## How to RETRIEVE (filesystem-first — no Obsidian, no MCP)
Full procedure: [`meta/workflows/retrieval.md`](meta/workflows/retrieval.md). In short:
1. **Search** with ripgrep over `notes/` (and `sources/` when you need evidence). Combine content terms with
   frontmatter filters, e.g.:
   - `rg -l -i "event decoupling" notes/`
   - `rg -l "^status: verified" notes/` · `rg -l "^type: concept" notes/` · `rg -l "^domain: <domain>" notes/`
2. **Prefer verified knowledge.** Rank `verified` > `reviewed` > `proposed`; treat `unreviewed`/`deprecated` with care.
3. **Read progressively.** Read a note's `summary` + frontmatter first; open the full body of only the 1–3 notes
   you actually need. Keep returned context small.
4. **Traverse relationships.** Follow `related:` and `[[wikilinks]]`. Find backlinks with
   `rg -l "\[\[<note-id>\]\]" notes/`.
5. **Cite.** When you use a claim, cite the note and its `sources:` entry.

## How to CONTRIBUTE
Pick the **proportional** path — do not run the full lifecycle for every scrap. Details in
[`meta/workflows/`](meta/workflows/).
- **Quick capture** → [`quick-capture.md`](meta/workflows/quick-capture.md): a link/idea/observation goes into
  `inbox/`. It **may stay there forever**; you are not required to make a note.
- **Source preservation** → [`source-preservation.md`](meta/workflows/source-preservation.md): preserve an original
  into `sources/` with provenance. **Do not auto-synthesize.**
- **Knowledge synthesis** → [`knowledge-synthesis.md`](meta/workflows/knowledge-synthesis.md): **search first**,
  decide **update vs create**, synthesize in your own words, add typed links, validate sources, set
  `status: proposed`, and **request human verification**.

**Always search before creating.** If a note already covers the topic, **update it** — do not create a duplicate.

## Governance — what you may and may not do
See [`meta/workflows/git-governance.md`](meta/workflows/git-governance.md). The rules:
- You may set `status:` only to the **entry states** for the note's type: `unreviewed`/`proposed` (claim-bearing) or
  `proposed` (`decision`). `source`/`index` have no status.
- **Only a human** sets the later states — `reviewed`/`verified`/`deprecated` (claim-bearing) or
  `accepted`/`rejected`/`superseded` (`decision`). Do not self-promote your own work.
  - `reviewed` = a human examined it; some validation may remain.
  - `verified` = a human confirmed the claims against traceable sources (passed the
    [validation checklist](meta/workflows/validation-checklist.md)) and approved it as reliable.
  - `accepted` (decision) = adopted — **not** a claim-verification state.
- Make changes on a **branch** and leave them as a reviewable **Git diff / PR**. Canonical knowledge enters `main`
  only through human review. **Git is the record of who-changed-what** — do not add a `last_modified_by` field.
- Keep `sources/` intact; corrections to synthesis go in `notes/`, not by rewriting a preserved source.

### Honesty about enforcement (important)
Folder-scoping rules in this file (e.g. "write only to `inbox/`") are **agent policy, not a technical control**.
Nothing in the filesystem or the optional MCP plugin *prevents* a broad write — the real safeguards are **human
review of Git diffs** and branch protection. Act within policy; rely on Git for governance.

## Working with Obsidian / MCP (optional)
If a human has Obsidian open with the Local REST API plugin, you may use its MCP tools for reading and searching
(see [`meta/mcp-setup.md`](meta/mcp-setup.md)). Caveats: it offers **lexical + metadata search only (no semantic
search)**, it is **unavailable when Obsidian is closed**, and its write tools are **not folder-scoped**. Therefore:
**prefer the filesystem for retrieval, and make all contributions via files + Git**, not MCP writes.

## When the filesystem is not enough
This template intentionally ships **no custom CLI, index, database, embeddings, or MCP server**. Propose adding one
**only** when you can show a *measured* failure of filesystem retrieval (see
[`meta/workflows/retrieval.md`](meta/workflows/retrieval.md) → "Signs the filesystem is not enough") — and justify
it in writing first. Do not add infrastructure for convenience or "more control" alone.
