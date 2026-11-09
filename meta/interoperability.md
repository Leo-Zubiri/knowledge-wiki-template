# Agent interoperability & adapters

This repository is **agent-agnostic**: the knowledge is plain Markdown + YAML + Git, and `AGENTS.md` is the one
canonical instruction file. This document states — **honestly, with sources** — how far that portability actually
reaches, and the two small optional adapters we ship. It does **not** claim universal compatibility.

## Four levels of interoperability
1. **Filesystem (the floor, universal).** Any agent that can read files can read/search/create/update the Markdown and
   YAML with zero integration. `AGENTS.md` and the workflows are plain prose; retrieval is `ripgrep`. This works with
   **Obsidian closed and MCP absent**, on every product below. This is the guarantee the repo rests on.
2. **Agent instruction discovery.** Whether a product auto-reads `AGENTS.md` — varies; see the matrix.
3. **Agent Skills (`SKILL.md`).** An optional way to package *procedures*; documented below, **not shipped**.
4. **Optional integrations.** Obsidian and its MCP server — see [`obsidian.md`](obsidian.md),
   [`mcp-setup.md`](mcp-setup.md). Always optional.

## Instruction-file support — classified (verified 2026-11-05)
Support falls into four classes. **Conditional support is not a guarantee** — version, surface, and config limits are
kept visible.

| Product (surface) | Support class | `AGENTS.md` discovery — with limits | Native file | Adapter we ship | Source |
|---|---|---|---|---|---|
| **OpenAI Codex** (CLI/cloud) | Native, unconditional | `[VERIFIED]` reads & merges `AGENTS.md`; 32 KiB combined cap (silent truncation); does not descend below cwd | `AGENTS.md` | none | learn.chatgpt.com/docs/agent-configuration/agents-md |
| **OpenCode** (CLI) | Native, unconditional | `[VERIFIED]` reads `AGENTS.md`; falls back to `CLAUDE.md` only if no `AGENTS.md` | `AGENTS.md` | none | opencode.ai/docs/rules |
| **Cursor** (IDE) | Native, conditional | `[VERIFIED]` reads `AGENTS.md`; `[CONDITIONAL]` known multi-root-workspace leak bug; `AGENTS.md` has no glob scoping | `.cursor/rules/*.mdc` | none | cursor.com/docs/context/rules |
| **Claude Code** (CLI) | Native, conditional → adapter | `[VERIFIED]` native since **v2.1.277**, default on, **but not universal**: gated by version & session type, built-in plugin is disablable, and **any `CLAUDE.md`/`CLAUDE.local.md` in scope suppresses `AGENTS.md`** | `CLAUDE.md` | **`CLAUDE.md` = `@AGENTS.md`** | code.claude.com/docs/en/memory |
| **GitHub Copilot** (cloud agent / VS Code / github.com) | Conditional / surface-dependent | `[CONDITIONAL]` docs allow `AGENTS.md` (nearest wins), but **which surfaces honor it, and its precedence vs `.github/copilot-instructions.md`, are `[UNVERIFIED]`** | `.github/copilot-instructions.md` | none | docs.github.com/en/copilot/how-tos/configure-custom-instructions/add-repository-instructions |
| **Gemini CLI** (CLI) | No native → adapter required | `[VERIFIED]` does **not** read `AGENTS.md` by default | `GEMINI.md` | **`.gemini/settings.json`** | geminicli.com/docs/cli/gemini-md |

**Honest summary:** `AGENTS.md` is read **natively and unconditionally by 2** products (Codex, OpenCode),
**natively but conditionally by 2** (Cursor, Claude Code), is **conditional/surface-dependent for 1** (Copilot), and
is **not read by 1** without an adapter (Gemini CLI). We do not claim "works everywhere."

## The two adapters we ship (both optional, non-load-bearing)
Neither adapter is part of the knowledge layer — remove either and the repo still works via the filesystem. Neither
**duplicates** `AGENTS.md`; both are a pointer/import.

1. **`CLAUDE.md`** — body is exactly:
   ```
   @AGENTS.md
   ```
   Claude Code's `@path` import loads `AGENTS.md` first; it is an **internal import** (no approval prompt) and
   **never double-loads** regardless of the Project-instructions setting. This makes `AGENTS.md` the single canonical
   source on **all** Claude Code versions/sessions, covering the conditional cases above.
2. **`.gemini/settings.json`** — points Gemini CLI at `AGENTS.md`:
   ```json
   { "context": { "fileName": ["AGENTS.md", "GEMINI.md"] } }
   ```

**No symlinks.** A `GEMINI.md → AGENTS.md` (or similar) symlink breaks on Windows (`core.symlinks=false` materializes
it as a text file containing the path) and some tools refuse to write through symlinks. Preference order: native →
`@import` → config pointer → one-line text pointer. **No other vendor files** are shipped for symmetry
(Codex/OpenCode/Cursor/Copilot need none).

## Retrieval: filesystem-first vs Obsidian MCP (verified capabilities)
For **finding knowledge**, the default is filesystem-first (see [`workflows/retrieval.md`](workflows/retrieval.md)). MCP
is an optional accelerator, **not** a retrieval upgrade — its search is lexical + metadata only (**no semantic search**),
capped, and offline when Obsidian is closed.

| Capability | Filesystem-first | Obsidian MCP (Local REST API) |
|---|---|---|
| Exact text search | **Excellent** (`ripgrep`, deterministic, precise for identifiers) | Good (`search_simple` fuzzy, capped) |
| Metadata filtering | Good (`rg` over frontmatter) | Good+ (`search_query` JsonLogic, structured) |
| Relationship discovery | **Good** (`related:` + backlink grep) | Uncertain (may not expose links/backlinks) |
| Progressive retrieval | **Full control** (list → read) | Helped by capped results |
| Live vault state | Files on disk (always) | Live Obsidian state (only while open) |
| Token efficiency | **High** (`-l` output is tiny) | High (capped payload) — tie |
| Security / permissions | **Simple** (OS / Git) | Bearer key + `.obsidian/` 403 + our deny-list; broad write exposed |
| Portability across agents | **Universal** (any file-capable agent) | Needs an MCP client + Obsidian |
| Maintenance | **None** | Plugin version, key, TLS, Obsidian running |

**Conclusion:** filesystem-first wins or ties on every retrieval axis; MCP's only unique edges (live state, UI actions)
are **not** retrieval. Use filesystem-first by default; reach for MCP only when a concrete non-retrieval capability
justifies it.

## Optionally exposing a workflow as an Agent Skill (`SKILL.md`)
Agent Skills are an **open spec** (agentskills.io; a folder with a required `SKILL.md` carrying `name` + `description`
frontmatter, with progressive disclosure). They are loaded by Anthropic products and OpenAI Codex (verified); broader
support is unconfirmed, and **discovery paths differ** (Claude: `.claude/skills/`; Codex: `.agents/skills/`).

**This template ships no `skills/` directory** — knowledge is declarative, Skills are procedural, and shipping skills
would duplicate the `meta/workflows/` and couple the repo to specific runtimes. If a domain repo *wants* to expose a
procedure (e.g. source-preservation) as a skill, use a **thin `SKILL.md` that points to the canonical workflow** —
never restate it:
```markdown
---
name: preserve-source
description: Preserve an external source into this knowledge repo with provenance. Use when the user adds a link,
  document, or quote to keep as evidence.
---
Follow the canonical workflow — the single source of truth:
see ../../meta/workflows/source-preservation.md . Do not restate the steps here; read and follow them.
```
Keep the procedure in one place (`meta/workflows/`); the skill is only a trigger + pointer. Note the relative path
only resolves inside this checkout — skills here are an in-repo convenience, not portable packages.

**Since template 1.1.0:** the optional [`playbook/`](../playbook/README.md) layer ships ≤ 7 such thin skills plus a
consumer bootstrap. In consumer projects, discovery uses **committed pointer skills** generated by
`playbook/setup.sh` (`.claude/skills/<x>/SKILL.md` for Claude Code/OpenCode, `.agents/skills/<x>/SKILL.md` for Codex)
— never symlinks, for the Windows reason above. Agents without skill discovery follow the pointer `AGENTS.md` →
`PLAYBOOK.md` → `SKILL.md` as prose. The matrix above is unchanged.

*All claims above verified 2026-11-05 against the cited official docs. `[UNVERIFIED]` items are surface/behavior that
the official docs do not pin down — treat them as conditional and test in your environment.*
