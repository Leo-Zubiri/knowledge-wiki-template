# Optional: connect an agent to Obsidian via MCP (quickstart)

This is an **optional** convenience that lets an agent **read and search** the vault through Obsidian **while it is
open**. It is **not required** — the default agent path is filesystem + Git (see
[`workflows/retrieval.md`](workflows/retrieval.md)), which works with Obsidian closed. Deleting `.mcp.json` disables it.

> **Posture: read/search only.** Agent *contributions* go through **files + Git**, never MCP writes. The repo ships a
> read-only permission set in [`../.claude/settings.json`](../.claude/settings.json). See [`security.md`](security.md).

## The plugin
- **`Local REST API`** by Adam Coddington (community id `obsidian-local-rest-api`; manifest name *"Local REST API with
  MCP"*). **Desktop-only.** As of **v5.5.2** it ships a **built-in MCP server** at `/mcp/` (Streamable HTTP) running
  inside Obsidian. **Pin ≥ 5.5.2** (see `security.md`). Do **not** use the archived `jacksteamdev/obsidian-mcp-tools`.

## 5-minute connect (Claude Code)
1. **Install & enable** in Obsidian: Settings → Community plugins → (disable Restricted Mode) → install *Local REST
   API* → enable. Copy the **API key** from its settings.
2. **Expose the key as an env var** (never put it in a file that's committed):
   ```bash
   export OBSIDIAN_API_KEY="paste-your-key"      # add to your shell profile, or use a local .env
   ```
   (`.env.example` is provided; copy to `.env` — gitignored — if you prefer a file.)
3. **Create the client config** from the example:
   ```bash
   cp .mcp.json.example .mcp.json                 # .mcp.json is gitignored (opt-in)
   ```
   It points Claude Code at `https://127.0.0.1:27124/mcp/` with `Authorization: Bearer ${OBSIDIAN_API_KEY}`
   (Claude Code expands `${OBSIDIAN_API_KEY}` at launch).
4. **TLS** — the plugin serves HTTPS on `:27124` with a *self-signed local CA*. Two options:
   - **Simplest:** enable the plugin's plain-HTTP port **`:27123` (localhost only)** and change the URL in `.mcp.json`
     to `http://127.0.0.1:27123/mcp/`. The bearer token then only crosses the loopback interface.
   - **Keep HTTPS:** trust the plugin's CA in your OS trust store (Claude Code's `CLAUDE_CODE_CERT_STORE` includes
     `system`); `NODE_EXTRA_CA_CERTS` pointing at the CA is a plausible alternative *(not verified for MCP here)*.
5. **Approve & verify:** start Claude Code in the repo; approve the `obsidian` server when prompted
   (`claude mcp reset-project-choices` re-prompts). Run **`/mcp`** to confirm it's connected and to see the **real tool
   names**. **Keep Obsidian open** — the server lives inside it (no headless mode).

**Other clients (provider-neutral):** the endpoint + bearer are generic. E.g. OpenAI Codex, in `~/.codex/config.toml`:
```toml
[mcp_servers.obsidian]
url = "http://127.0.0.1:27123/mcp/"
bearer_token_env_var = "OBSIDIAN_API_KEY"
```

## Read-only enforcement
[`../.claude/settings.json`](../.claude/settings.json) **allows** only the read/search tools and **denies** the write/
privileged ones by exact name. `[FACT]` In Claude Code, `permissions` is the enforced control (precedence **deny > ask
> allow** — an allow can't override a deny); a skill's `allowed-tools` only *grants*, it doesn't restrict. Confirm the
actual tool names with `/mcp` and adjust the lists if the plugin changed them.

## What the MCP server offers
- **Read/search (use these):** `vault_read`, `vault_list`, `vault_get_document_map`, `search_simple` (fuzzy
  full-text), `search_query` (**JsonLogic** over frontmatter/tags/path/content), `tag_list`.
- **Write (prefer files+Git instead):** `vault_write`, `vault_append`, `vault_patch`, `vault_move`, `vault_copy`,
  `vault_delete`.
- **Privileged (leave denied):** `command_execute`/`command_list`, signed-URL minting.

## Known limitations (plan around them)
- **No semantic search** — lexical + metadata only. Good queries + structured frontmatter matter.
- **MCP search is capped** (≈20 files / 10 matches / 500-char snippets) — good for context budget. (Raw REST is
  uncapped → prefer the MCP tools.)
- **Backlinks over MCP are uncertain** — our greppable `related:` field is the portable fallback.
- **Offline floor** — Obsidian closed = no MCP. This is exactly why agents are filesystem-first.
- **No folder-level write enforcement** — the plugin exposes broad vault writes; "write only to inbox" is *policy*,
  not a control. Governance is human Git review (see [`workflows/git-governance.md`](workflows/git-governance.md)).
