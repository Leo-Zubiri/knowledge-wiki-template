# Optional: Local REST API (with MCP) setup

This is an **optional** convenience that lets agents read and search the vault **through Obsidian while it is open**.
It is **not required** — the default agent path is filesystem + Git (see
[`workflows/retrieval.md`](workflows/retrieval.md)), which works with Obsidian closed.

> **Recommended posture for the MVP: read/search only.** Make all agent *contributions* via files + Git, not MCP
> writes. Reasons are in [`security.md`](security.md) and below.

## The plugin
- **`Local REST API`** by Adam Coddington — community plugin id `obsidian-local-rest-api`. Its manifest name is
  literally *"Local REST API with MCP"*. It is **desktop-only** and, as of **v5.5.2** (2026-10-17), **ships a
  built-in MCP server** at `/mcp/` (Streamable HTTP) running inside Obsidian.
- **Do not** use the old `jacksteamdev/obsidian-mcp-tools` companion — it was **archived in 2026-05-13**. Most blog
  posts describing "REST + MCP" predate the built-in server and are stale.
- `cyanheads/obsidian-mcp-server` is an actively maintained third-party alternative; consider it only if you need
  remote/multi-client MCP auth (JWT/OAuth) or BM25 (Omnisearch). Not needed for the default read-only setup.
- **Pin version ≥ 5.5.2** (see [`security.md`](security.md) for why).

## Install & connect (high level)
1. In Obsidian: Settings → Community plugins → disable Restricted Mode → install **Local REST API** → enable.
2. Copy the **API key** from the plugin settings.
3. Note the endpoints: HTTPS on `https://127.0.0.1:27124` (local CA) — MCP at `https://127.0.0.1:27124/mcp/`. An
   optional plain-HTTP port `27123` exists but is **off by default**; leave it off.
4. Point your agent's MCP client at the `/mcp/` endpoint with `Authorization: Bearer <API key>`.
5. **Keep Obsidian open** — the server lives inside it (no headless mode).

## What the MCP server actually offers
- **Read/search tools** (use these): `vault_read`, `vault_list`, `vault_get_document_map`, `search_simple` (fuzzy
  full-text), `search_query` (**JsonLogic** over frontmatter/tags/path/content), `tag_list`.
- **Write tools** (exist, but prefer files+Git): `vault_write`, `vault_append`, `vault_patch` (by heading/block/
  frontmatter), `vault_move`, `vault_copy`, `vault_delete`.
- **Privileged** (leave disabled): `command_execute`/`command_list` (arbitrary command palette) and signed-URL minting.

## Known limitations (plan around them)
- **No semantic search** — lexical + metadata only. Good queries and structured frontmatter matter.
- **MCP search is capped** (≈20 files / 10 matches / 500-char snippets) — this is *good* for context budget. (Raw
  REST is uncapped — prefer the MCP tools.)
- **Backlinks over MCP are uncertain** — it may not return link/backlink fields. Our greppable `related:` field is
  the portable fallback.
- **Offline floor** — closed Obsidian means no MCP. This is exactly why agents are filesystem-first.
- **No folder-level write enforcement** — the plugin exposes broad vault writes; "write only to inbox" is *policy*,
  not a control. Governance is human Git review (see [`workflows/git-governance.md`](workflows/git-governance.md)).
