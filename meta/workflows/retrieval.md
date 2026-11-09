# Workflow: Filesystem-first retrieval

Find and read knowledge using only ordinary tools — **no Obsidian, no MCP, no index, no custom engine**. This is the
**default** retrieval path; it works with Obsidian closed and is portable to any agent. (Why filesystem-first beats
MCP for retrieval: see the comparison in [`../interoperability.md`](../interoperability.md).)

> **Cost principle:** the expensive part is the text pulled **into the model's context**, not running the search.
> **Return little; read progressively.** Running `rg` costs no context; only what you *read* does.

## Required capabilities (provider-neutral)
Any agent can do this with: **recursive text search** (`ripgrep` preferred; `grep -r` or any substring search works),
**read a file**, **list a directory**, and **git**. Commands below use `rg`; substitute your tool's equivalent.

## The progressive protocol
1. **Understand the question** — identify domain(s), key terms, and intent (point fact / comparison / survey).
2. **Discover candidates** by metadata + filenames, not by reading bodies:
   `rg -l "^domain: <domain>" notes/` · `rg -l "^tags:.*<facet>/<value>" notes/` (if the repo uses [tags](../methodology/tags.md); a search aid, not evidence) · match filenames · check `index`/MOC notes.
3. **Filter** by dimension (intersect file lists):
   ```sh
   comm -12 <(rg -l '^status: verified' notes/|sort) <(rg -l -i '<topic>' notes/|sort)
   ```
4. **Search content** with exact terms **and synonyms/related terms you generate** (compensates for lexical-only
   search): `rg -n -C1 -i '<term>' notes/` for snippets.
5. **Inspect a few** — read **frontmatter + `summary`** of top candidates *before* opening any full body.
6. **Follow relationships** only for the 1–3 that matter: `related:` + `[[wikilinks]]`; backlinks =
   `rg -l "\[\[<id>\]\]" notes/`. Inverse relations are **derived here**, not stored (see
   [`../methodology/relationships.md`](../methodology/relationships.md)).
7. **Read `sources/`** only when a claim needs its original evidence.
8. **Prefer `verified`** but **don't hide `proposed`** — surface it with its status as a trust signal.
9. **Answer with references** — cite the note `id` and its `sources:` entry.
10. **State insufficiency** — if the repo lacks evidence, say so; never fabricate.

## When to STOP (don't over-search / over-read)
- You have 1–3 notes that answer the question with citations → stop.
- A `verified` note already answers it → stop (don't keep scanning).
- Two full passes with different terms find nothing relevant → **report "not enough evidence"**, don't keep loading files.
- Never read the whole vault "to be sure."

## Keeping a good answer (optional, only when asked)
If a human asks to keep a synthesized answer, write it as a **new or updated note via
[knowledge-synthesis](knowledge-synthesis.md)**, not as a summary of other notes. Its `sources:` must point at the
**original** `sources/` entries (or URLs) the answer rests on — a note that cites only other notes has no provenance of
its own. Set `status: proposed` and leave verification to a human. Do not file answers automatically.

## Handling specific cases
- **Exact keyword** (API names, error strings) → `rg` directly; its strongest case.
- **Conceptual / different terminology** → expand with synonyms; lean on `summary`, **`aliases`** (frontmatter), and
  `index`/MOC notes. *(This is the real limit of lexical search — and MCP shares it; structure, not MCP, is the fix.)*
- **Related notes** → `related:` + backlink grep (step 6).
- **Contradictions** → look for `contradicts` links and `anti-pattern` notes; present both sides with their statuses.
- **Missing metadata** → fall back to full-text + filename; flag the gap.
- **Large repos** → `-l`/`head`, two-phase read, `index`/MOC as entry points. Only a *measured* failure (latency, or
  needing one overview instead of many greps) would justify a generated catalog — see "Signs the filesystem is not
  enough" below. No DB/embeddings.
- **Multi-domain** → filter per `domain`, or search across and surface `domain` in results.

## Token efficiency — what costs context
- **Tool execution** (`rg`, `ls`, `git`): CPU/disk, **not** model context.
- **Tool output** (stdout): controllable — `-l` (filenames only), `-n -C1` (small snippets), `head`.
- **Model context**: only what you read/return — the protocol keeps it to a few notes.
- **Reasoning context**: the 1–3 right notes, not the vault.

## (Optional) via Obsidian MCP, when Obsidian is open
If a human runs Obsidian with the Local REST API plugin, you *may* use `search_simple`/`search_query`/`vault_read`
(see [`../mcp-setup.md`](../mcp-setup.md)). It is **lexical + metadata only (no semantic search)**, **capped**, and
**offline when Obsidian is closed** — so it offers **no retrieval advantage** over the filesystem and remains an
optional accelerator, never the default. Prefer the filesystem.

## Signs the filesystem is NOT enough (when to propose more)
Only a **measured, repeated** failure justifies revisiting the "not building" list:
- relevant notes consistently missed by lexical search; poor recall across terminology despite `aliases`;
- indirect relationships you can't surface via links/backlinks; excessive context per query despite two-phase reads;
- a hard need to retrieve **without Obsidian** that the filesystem genuinely can't meet.
Write it up with examples and propose the **smallest** addition — never infrastructure for convenience.

## Validating this protocol on your own corpus (optional)
To check that filesystem-first retrieval works well for your domain, run a small evaluation on a real `<domain>-knowledge`
repo rather than assuming results. Seed a representative sample (~10–20 notes — a mix of `verified` and `proposed`,
including at least one deliberate contradiction). Then, for a set of representative questions, compare retrieval
strategies (metadata→read vs. ripgrep-progressive vs. MCP, where available) on: relevance of the notes retrieved, answer
correctness, source traceability, number of files inspected, content returned to the model, tool calls, and approximate
tokens. Use what you measure to decide whether the filesystem protocol meets your needs before adding any tooling (see
"Signs the filesystem is NOT enough" above).
