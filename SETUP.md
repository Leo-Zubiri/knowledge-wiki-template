# SETUP — getting a domain knowledge repo ready (agent-assisted)

This is the setup checklist. **Ask your agent "help me set up this knowledge repo"** and it will do the file-based steps
and guide you through the GUI/plugin steps. Or do it manually with the commands below. Everything here is **optional
layering** — the repo already works as plain Markdown + Git.

> Honest split: an agent can create/repair files and run Git. It **cannot** click in Obsidian's UI, install a browser
> extension, or paste your API key — those stay with you (by design/security).

## What the AGENT can do for you (files + Git)
- Confirm/repair the minimal `.obsidian/` config (`core-plugins.json` enabling Templates/Bases/Graph/Backlinks/Daily
  notes/Properties; `templates.json` → `meta/templates`). *(These are best-effort, undocumented Obsidian internals —
  the agent regenerates them for your installed version if they drift; the repo works even if `.obsidian/` is deleted.)*
- Place/keep the Bases dashboards in `dashboards/`.
- Prepare MCP config: `cp .mcp.json.example .mcp.json` (gitignored). (You still paste the key — see below.)
- Set your `domain:` convention and show examples; run the validation checks.
- Draft notes via the [intake workflow](meta/workflows/intake.md) once you give it material.

## What YOU do (GUI / security / browser — the agent guides)
1. **Open the folder as a vault** in Obsidian.
2. **Core plugins** should already be on (from `.obsidian/core-plugins.json`). If not: Settings → Core plugins → enable
   Templates, Bases, Graph, Backlinks, Daily notes, Properties. Templates folder → `meta/templates`.
3. **(Optional) Community plugins** — install + enable manually (code isn't shipped; **Restricted Mode blocks them by
   default**): *Local REST API* (for MCP). Optionally *QuickAdd*/*Templater* for faster capture. Turn off Restricted
   Mode only if you trust the authors.
4. **(Optional) MCP** — in *Local REST API* copy the **API key**; `export OBSIDIAN_API_KEY="…"`; verify with `/mcp`.
   Full steps + read-only posture: [`meta/mcp-setup.md`](meta/mcp-setup.md).
5. **(Optional) Web Clipper** — install the browser extension; import
   [`meta/web-clipper-template.json`](meta/web-clipper-template.json) (drag it into the clipper's template area) and set
   its destination folder to `inbox/`. (Adjust fields to your clipper version — see [`meta/obsidian.md`](meta/obsidian.md).)

## Then
Feed it ([intake](meta/workflows/intake.md)) → review & verify in Git ([git-governance](meta/workflows/git-governance.md))
→ retrieve ([retrieval](meta/workflows/retrieval.md)). End-to-end overview: [`QUICKSTART.md`](QUICKSTART.md).
