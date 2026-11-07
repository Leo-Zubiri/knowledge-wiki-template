# Overview — a guided tour of this knowledge repository

**New here? Read this first.** It's the friendly, end-to-end explanation of *what this repo is, why it's shaped this
way, and what using it feels like*. It is a **tour, not a spec**: every detail lives canonically in `meta/`, and this
page links there instead of restating it (that "reference, don't duplicate" discipline is a core rule of the repo —
see [`meta/specialist.md`](meta/specialist.md)). For the terse architecture intro, see [`README.md`](README.md); to
start setting up, [`QUICKSTART.md`](QUICKSTART.md); if you're an agent, [`AGENTS.md`](AGENTS.md).

---

## 1. What it is, in one minute

A **template** for a knowledge base that is, at the same time, a **"second brain" for humans** and an **agent-readable
"LLM wiki"**, where **all knowledge lives as Markdown + YAML inside Git**, every claim **cites its source**, and every
note **links** to the related ones.

It is **not** an app. There is no database, no index, no server, no embeddings, no bespoke CLI. **The filesystem *is*
the product.** You clone it to start a knowledge base for one domain (`laravel-knowledge`, `architecture-knowledge`,
…): the template gives you the structure, conventions and rules; each domain repo holds its own knowledge.

## 2. The problem it solves

Teams pile knowledge into tools that **lock it in**, **can't be read by agents**, **lose provenance** ("where did this
come from?"), and **blur a raw observation with a verified fact**. This template answers all four: knowledge in
**portable files + Git**, **per-claim provenance**, and a strict **separation of observation from verified knowledge**.

What it is **not**: not an app/DB/index/RAG, not tied to one AI vendor or to Obsidian (all optional), and **not an
agent's session memory** — it is durable, reviewed, shared knowledge.

## 3. The big idea: layered, not a flat menu

The mistake this design avoids is treating every methodology as an interchangeable "architecture". Here **each
methodology owns exactly one layer**. Full table and diagram in [`README.md` §4](README.md); the reasoning in
[`meta/methodology/methodologies.md`](meta/methodology/methodologies.md). In short:

- **CODE** → the information **lifecycle** (how knowledge is produced & maintained).
- **LLM-Wiki** → **provenance, synthesis discipline, verification** (how much you trust it).
- **Zettelkasten** → **typed, reason-annotated links** (how notes connect).
- **PARA** → an **optional** task layer only — deliberately *not* applied to the knowledge core.
- Underneath: **Markdown + YAML + wikilinks** (format), **filesystem + Git + progressive disclosure** (access),
  **Obsidian · MCP · instruction files** (optional interfaces).

They're orthogonal (time, trust, graph, actionability), so they stack without fighting.

## 4. How knowledge is stored: three content classes

Physically separated into folders, with different trust and mutability — so a bad synthesis can always be re-derived
from a preserved original. Details: [`meta/methodology/lifecycle.md`](meta/methodology/lifecycle.md).

| Folder | What it holds | Trust | Mutability |
|---|---|---|---|
| [`inbox/`](inbox/) | quick capture (links, ideas, snippets) | untrusted, **never cited** | mutable |
| [`sources/`](sources/) | preserved originals + provenance | evidence (fidelity to original) | **immutable by convention** |
| [`notes/`](notes/) | the wiki: own-words, cited, linked notes | knowledge (grows by review) | edited by consolidation |

`inbox/` items **may live there forever** — a capture never *has* to become a note. `notes/` is **what you read to
answer a question**.

## 5. Every note's metadata: three independent dimensions

Each note carries **portable YAML frontmatter** (parseable without Obsidian). The trick is keeping three dimensions
apart — full field reference in [`meta/methodology/metadata.md`](meta/methodology/metadata.md):

1. **`type`** (what it is) — one of eight; pick with [`meta/methodology/note-types.md`](meta/methodology/note-types.md):
   `source · concept · pattern · anti-pattern · reference · guide · decision · index`.
2. **`status`** (how trusted) — **scoped by type** (see §6).
3. **Location** (where it lives) — the **folder**, not a field, so it can never disagree with reality.

Two honest choices worth knowing: there is **no `last_modified_by`** (Git already records that), and
**`created_by_type: human|agent` is author-provenance, *not* a trust signal** — trust comes from `status` + sources +
human review, never from who wrote it.

Knowledge connects two ways, and they're kept separate on purpose:
- **Provenance** (note → its evidence) lives in `sources:` — each entry ties **one claim** to a `url` or a local `ref`.
- **Relationships** (note → note) live in `related:` — **typed and reason-annotated** ("every link says *why*"). You
  store **one canonical direction** (`refines`, `depends-on`, `contradicts`, …) and the inverse is **derived** at
  retrieval, never hand-written. Taxonomy: [`meta/methodology/relationships.md`](meta/methodology/relationships.md).

## 6. The heart of the method: lifecycle & review states

The **non-negotiable rule**: *an agent observation never silently becomes verified knowledge.* The chain is always
preserved — original → raw input → agent observation → proposed synthesis → reviewed → verified.

**Status ladders are scoped by type** (full definitions in
[`meta/methodology/lifecycle.md`](meta/methodology/lifecycle.md)):

- Claim-bearing notes (`concept/pattern/reference/anti-pattern/guide`):
  `unreviewed → proposed → reviewed → verified → deprecated`.
- Decisions (`decision`, ADR semantics): `proposed → accepted → superseded` (or `rejected`). Note `accepted` means
  "the decision was made", **not** "every claim is verified".
- `source` and `index` carry **no trust status**.

**Who may change what:** an agent may set only the entry states (`unreviewed`/`proposed`). **Only a human** promotes to
`reviewed`/`verified`/`deprecated` (or `accepted`/`rejected`/`superseded`), through Git review.

## 7. Contributing: three proportional paths

Guiding bias: **"capture is cheap; synthesis is deliberate"** and **"always search before creating"** (if a note
already covers it, you *update* it — you don't duplicate). Match the effort to the material:

1. **[Quick capture](meta/workflows/quick-capture.md)** → `inbox/`. Minimal effort; may stay forever.
2. **[Source preservation](meta/workflows/source-preservation.md)** → `sources/`. Keep an original as evidence, with
   provenance. **No synthesis here.**
3. **[Knowledge synthesis](meta/workflows/knowledge-synthesis.md)** → `notes/`. Search first → update-or-create →
   write in your own words → cite each claim → add typed links → `status: proposed` → **ask a human to verify**.

The **[intake loop](meta/workflows/intake.md)** ties them together (triage → classify → search → preserve → dedupe /
flag contradictions → synthesize → validate → hand off on a branch), with a route-by-input-type table (URL, PDF, code
repo at a pinned commit, chat log, dataset, your own idea…).

## 8. Retrieving: filesystem-first

Default retrieval uses only ordinary tools (`ripgrep` + read + `git`), works with **Obsidian closed**, and is portable
to any agent. The cost principle: *what's expensive is the text pulled into the model's context, not running the
search* — so **return little, read progressively**. The protocol (metadata/filenames → filter → content search with
synonyms → read `summary` before bodies → follow links for the 1–3 that matter → open `sources/` only if needed →
answer with citations → **know when to stop**) lives in [`meta/workflows/retrieval.md`](meta/workflows/retrieval.md).

## 9. Governance: Git + human review (and honesty about enforcement)

The most honest point in the whole repo: rules like "agents write only to `inbox/`" are **policy, not a technical
control** — nothing in the filesystem or MCP enforces folder-level write scoping. The safeguard that actually holds is
**human review of Git diffs**. Agents propose on a **branch**; a human reviews the diff, runs the
[validation checklist](meta/workflows/validation-checklist.md), and promotes the status; **canonical knowledge enters
`main` only through review**. Full model (and the four mechanisms not to confuse) in
[`meta/workflows/git-governance.md`](meta/workflows/git-governance.md).

## 10. Interoperability, honestly

`AGENTS.md` is the one canonical instruction file, and the repo is **agent-agnostic — but does not claim "works
everywhere".** The only universal guarantee is **the filesystem** (any agent that can read files can use the repo).
Whether a given product auto-reads `AGENTS.md` varies, so the repo ships two small **optional** adapters
(`CLAUDE.md = @AGENTS.md`, and `.gemini/settings.json`). The verified support matrix is in
[`meta/interoperability.md`](meta/interoperability.md).

## 11. Optional layers (never required)

The repo is fully usable as plain Markdown + Git. On top, all optional:
- **Obsidian** ([`meta/obsidian.md`](meta/obsidian.md)) — a human UI for editing, Graph, and dashboards; everything it
  shows is derived from the files. The repo works even if you delete `.obsidian/`.
- **Dashboards** ([`dashboards/`](dashboards/)) — no-code Bases views over your frontmatter (e.g. unverified queue,
  missing provenance).
- **MCP** ([`meta/mcp-setup.md`](meta/mcp-setup.md)) — read/search over MCP *while Obsidian is open*. Honestly: it's
  **no retrieval upgrade** (lexical only, capped, offline when Obsidian is closed) — filesystem-first stays the default.
- **The specialist** ([`meta/specialist.md`](meta/specialist.md)) — a guidance role (its Claude Code / Codex adapter is
  the thin `.claude/skills/knowledge-ops/` skill). It *prepares* changes but **never marks anything `verified`.**
- **Consuming this repo from another project** ([`meta/workflows/cross-repo-consumption.md`](meta/workflows/cross-repo-consumption.md))
  — let a separate project repo read this knowledge base (read-only) and stage reusable finds back; domain-agnostic, no new infra.

**On adding infrastructure:** the template ships no CLI/DB/index/embeddings/MCP on purpose. Propose one only after a
*measured, repeated* failure of filesystem retrieval — and the smallest possible addition. Never for convenience.

## 12. What using it feels like, end to end

1. **Set up** ([`QUICKSTART.md`](QUICKSTART.md) / [`SETUP.md`](SETUP.md)) — clone into `<domain>-knowledge`, pick your
   `domain:`, optionally wire Obsidian/MCP/the specialist. An agent can do the file steps; you do the GUI/key steps.
2. **Feed it** — drop material into `inbox/`, or ask an agent *"ingest this URL"* / *"process my inbox"*.
3. **Process** — the intake loop preserves originals to `sources/` and drafts `proposed` notes in `notes/`, on a branch.
4. **Review & publish** — *you* review the Git diff, run the checklist, and promote `proposed → verified`. It lands on
   `main`.
5. **Retrieve** — you or an agent search progressively and answer **with citations**.
6. **Maintain** — update notes (Git keeps history), link contradictions, `deprecate` what's outdated.

**True at every step:** Markdown + Git is the source of truth; Obsidian, MCP and the specialist are optional; **nothing
is `verified` without a human**; and no secrets in Git (`.mcp.json`/`.env` are gitignored).

---

### One-line summary

A **file-based, Markdown-native, source-grounded, graph-linked** knowledge system with **progressive disclosure and
Git-based governance**, usable **at once by humans and AI agents**, whose only hard guarantee is the filesystem and
whose only door to "verified knowledge" is **human review**.

> Where to go next: [`README.md`](README.md) (architecture intro) · [`AGENTS.md`](AGENTS.md) (agent entry point) ·
> [`meta/methodology/`](meta/methodology/) (the "why") · [`meta/workflows/`](meta/workflows/) (the "how").
