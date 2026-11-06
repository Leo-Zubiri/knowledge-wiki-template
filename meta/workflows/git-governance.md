# Workflow: Git-based governance & review

Git is the **governance of record** — the real audit trail, review surface, and rollback mechanism. This document
separates four things that are easy to confuse.

## Four distinct mechanisms (do not conflate them)
| Mechanism | What it actually is | What it can enforce |
|---|---|---|
| **Technical enforcement** | OS file permissions; Git branch protection; the MCP plugin's `.obsidian/` 403 | real, machine-enforced limits (e.g. protected `main`; optional read-only `sources/`) |
| **Agent policy** | instructions in `AGENTS.md`/workflows ("write only to `inbox/`", "set only `proposed`") | **guidance only — not a security boundary**; an agent *can* exceed it |
| **Human governance** | a human reviews proposed changes; only a human mints `verified` | trust and publication decisions |
| **Git protection** | diffs, history, revert, required PR review, protected branches | audit, rollback, gating canonical changes |

**Key honesty:** neither the filesystem nor the optional Local REST API plugin enforces folder-level write scoping.
"Agents write only to `inbox/`" is **policy**. The safeguard that actually holds is **human review of Git diffs**.

## The review model
1. **Agents (and humans drafting) work on a branch.** Make edits as files; commit with clear messages.
2. **Open the branch for review** (a PR, or a local diff a human inspects).
3. **A human reviews the diff.** They check synthesis quality, citations, links, and that nothing clobbered
   `sources/` or existing verified notes.
4. **Promotion happens in review:** a human sets `status: reviewed` or `verified` (with a resolvable `sources:`
   citation for `verified`). Agents never self-promote.
5. **Merge to `main`.** `main` is canonical; it changes **only** through reviewed diffs.

## Recommended protections (set per repo)
- Protect `main` (require review before merge) if your host supports it.
- Optionally make `sources/` read-only at the OS level if you want *technical* immutability (otherwise it is
  immutable-by-convention + review).
- Keep commits small and topical so diffs stay reviewable.

## Status-change authority (summary)
| status | who may set it |
|---|---|
| `unreviewed`, `proposed` | agents or humans |
| `reviewed`, `verified`, `deprecated` | **humans only**, via review |

## Concurrency
There is no CRDT/auto-merge for prose. When humans and agents may both write, prefer **single-writer-at-a-time** or
**short-lived branches** to keep merges to ordinary (rare) text conflicts.

## What Git replaces
You do **not** need a `last_modified_by` frontmatter field — `git log`/`git blame` already answer "who changed what,
when". The optional MCP plugin's ETag/trash features help a little, but **do not** replace commit-level review.
