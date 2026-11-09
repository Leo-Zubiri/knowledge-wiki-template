# SETUP — getting a domain knowledge repo ready (agent-assisted)

This is the setup checklist. **Ask your agent "help me set up this knowledge repo"** and it will do the file-based steps
and guide you through the GUI/plugin steps. Or do it manually with the commands below. Everything here is **optional
layering** — the repo already works as plain Markdown + Git.

> Honest split: an agent can create/repair files and run Git. It **cannot** click in Obsidian's UI, install a browser
> extension, or paste your API key — those stay with you (by design/security).

## Done when… (the minimum that counts as "set up")
You do not need Obsidian, MCP or the clipper to be finished. The repo is ready when all of these are true:
- [ ] You cloned it into `<domain>-knowledge` with fresh history, and chose your `domain:` value ([QUICKSTART](QUICKSTART.md) §1).
- [ ] At least one original is preserved in `sources/` and at least one note in `notes/` cites it by `ref` with
      `status: proposed` (see [`examples/`](examples/) for a tiny pair to imitate).
- [ ] The [validation recipe](meta/workflows/validation-checklist.md#copy-paste-check-recipe) prints `OK` (or you know
      which problems it reports).
- [ ] The work sits on a branch as a Git diff; **a human** decides whether anything becomes `verified`.

## Minimal profile (what you can ignore at the start)
Start with `inbox/` → `sources/` → `notes/`, YAML frontmatter, one `sources:` entry per claim, and `status: proposed`.
Everything else is **optional layering you add when a need appears**: tags, typed `related:` links beyond a plain
"See also", the Bases dashboards, MCP, the specialist skill, the `reviewed` step. The full rules stay in
[`meta/`](meta/README.md); you only open a page when you hit its situation.

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
