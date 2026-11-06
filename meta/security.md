# Security & operational considerations

This repo is, by default, just files + Git — low attack surface. Risk is introduced only when you enable the optional
Obsidian Local REST API (with MCP) plugin. Treat this document as the checklist before doing so.

## Enforcement vs policy (read this first)
Four different things protect this knowledge base; **do not confuse them** (full table in
[`workflows/git-governance.md`](workflows/git-governance.md)):
- **Technical enforcement** — OS permissions, Git branch protection, the plugin's `.obsidian/` block. *Real.*
- **Agent policy** — instructions in `AGENTS.md`/workflows. *Guidance only, not a boundary.*
- **Human governance** — review of changes; humans alone mint `verified`.
- **Git protection** — diffs, history, revert, required review. *The governance of record.*

**Key point:** neither the filesystem nor the plugin enforces "agents write only to `inbox/`". That is **policy**;
the real safeguard is **human review of Git diffs**.

## Secrets
- The Local REST API **key** (and other plugin secrets) are stored in plaintext at
  `.obsidian/plugins/<id>/data.json`. The provided `.gitignore` excludes this — **never commit it.**
- Also excluded: `.obsidian/workspace*.json` (churn, no secrets but noisy).

## Local REST API plugin — transport & auth
- Auth: **API key** as `Authorization: Bearer <key>`.
- Transport: **HTTPS on :27124** with a locally generated CA (scoped to localhost). An optional plain-HTTP **:27123**
  is **off by default** — leave it off.
- `.obsidian/` reads/writes are **refused (HTTP 403)** by default; keep that default (an override exists — don't use it).
- Hardening present: brute-force throttle, short-lived signed URLs, ETag/If-Match conditional writes, delete→trash.
- Leave `command_execute` and signed-URL minting **disabled** unless a specific, vetted need arises.

## Version & known advisories (verified 2026-11-05)
**Pin version ≥ 5.5.2.** As of v5.5.2 (2026-10-17) there are **3 published advisories, and all are patched — v5.5.2
is safe against all three** (sources: the repo's `/security/advisories` and the GitHub advisory API/release notes):

| Advisory | Class | Severity | Affected | Fixed in |
|---|---|---|---|---|
| GHSA-62gx-5q78-wrvx | path traversal via `%2F` in `/vault/{path}` | High (CVSS 8.8) | ≤ 4.1.2 | **4.1.3** |
| GHSA-j9qv-qgpv-345g | path traversal in MCP `vault_write` | High | 5.1.0 | **5.2.0** |
| GHSA-66m9-r757-qvq7 | RCE via config-directory write | High | 5.3.1 | **5.4.0** |

(GitHub advisories have no "open" state — only *draft/published/withdrawn*. "Published" ≠ "unpatched".) Re-check the
advisories page before each upgrade. A noted residual limitation (symlinked folders pointing outside the vault) is
worth awareness **if you enable MCP writes**.

## Operational notes
- **Offline:** the MCP/REST server runs inside Obsidian; closing Obsidian removes that access entirely. Agents must
  therefore be able to work from files + Git (they are, by design).
- **Concurrency:** no automatic merge for prose — prefer single-writer-at-a-time or short-lived branches when humans
  and agents may both write; resolve any conflicts as ordinary Git text merges.
- **`sources/` immutability** is by **convention + Git review**, not a technical lock. Add OS read-only permissions
  only if a domain repo specifically wants enforced immutability.
- **Recovery:** Git is the backstop — `git log`/`git revert` undo bad changes; protect `main` with required review.
