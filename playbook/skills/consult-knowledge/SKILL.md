---
name: consult-knowledge
description: Retrieve knowledge from the knowledge base with minimal tokens (search metadata and terms, read 1-3 notes, optional MCP accelerator) and decide whether a gap exists. Use before any non-trivial domain decision or when a note id is needed for a citation.
---

# Consult the knowledge base

Base path: the project's `AGENTS.md` says where (`$<KB_ENV_VAR>` or the sibling folder). Full protocol:
`meta/workflows/retrieval.md` in the base.

1. **Terms.** 2–3 terms in the base's language (ids, tags and summaries).
2. **Search.** `rg -l -i "<term>" <base>/notes/`; filter by frontmatter when useful (`rg -l "^status: verified"`,
   `rg -l "^type: pattern"`). If an Obsidian MCP is configured and the vault is open, its `search_simple` /
   `search_query` are a capped, cheaper alternative — never its write tools.
3. **Read 1–3 notes.** `summary:` and frontmatter first (`sed -n '1,40p'`); the body only if it answers the
   question. Follow `related:` for at most one hop.
4. **Trust.** `verified` > `reviewed` > `proposed` > `unreviewed`; state the status when citing. `sources:` refs
   are evidence; `inbox/` is never citable.
5. **Cite** the note id in reasoning, ADRs and commit messages.
6. **Stop rule.** Two searches with different terms and nothing sufficient → declare a gap: if generic, run
   `write-back-knowledge`; if project-specific, answer from bounded research and record it in the project's docs.

Never load the whole base. Never cite what you did not open.
