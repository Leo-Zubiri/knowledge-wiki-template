# Obsidian configuration guidance (optional, for humans)

Obsidian is the **human** workspace — editing, navigation, Graph, and dashboards. It is **optional**: the repo is a
plain folder of Markdown, so agents and humans can use it with no Obsidian at all. Everything Obsidian computes
(properties, links, backlinks, tags, graph, search) is derived from the files.

## Opening the vault
Open the repository folder in Obsidian as a vault. The template ships a **minimal `.obsidian/`** that **pre-enables the
core plugins** (Templates, Bases, Graph, Backlinks, Daily notes, Properties) and points **Templates** at
`meta/templates/`, so it opens mostly ready.
> **Caveat (honest):** the `.obsidian/*.json` schema (except `workspace*.json`) is **undocumented and version-fragile** —
> Obsidian may rewrite it. Treat the shipped config as best-effort; if something isn't enabled, flip it in Settings (or
> ask the agent to repair it). The repo works even if you delete `.obsidian/` entirely. Per-user bits
> (`workspace*.json`, caches, plugin `data.json`) are gitignored.

## Low-friction capture into `inbox/`
You don't have to hand-create each note. Capture, then process later (see
[`workflows/intake.md`](workflows/intake.md)):
- **Web Clipper** (official browser extension): clips a page as Markdown into a **configurable folder**. Import
  [`web-clipper-template.json`](web-clipper-template.json) (drag it into the clipper's template area) and set its
  destination to `inbox/`. *(The clipper's exact field names vary by version — adjust after import; the key settings are
  **destination folder = `inbox/`** and the capture **properties**.)*
- **Mobile** (native, no plugin): the **Share Sheet** and **Quick Capture** can send to a chosen folder — point them at
  `inbox/`.
- **Just drop a file**: any `.md`/PDF placed in `inbox/` works.
- **Properties without a template**: the Properties panel (`Cmd/Ctrl+;`) adds YAML frontmatter to any note — templates
  are a convenience, not a requirement.

## Use native, file-derivable features only
To keep knowledge portable, rely on features that are just Markdown/YAML under the hood:
- **Properties** = the YAML frontmatter we already use (`type`, `status`, `domain`, …).
- **Wikilinks & backlinks** = our `[[...]]` links and the `related:` field.
- **Graph / Local Graph** = navigate the knowledge web (human insight; agents use the links directly).
- **Search** = Obsidian's full-text search mirrors what `ripgrep` does on the files.
- **Templates (core plugin)** = point the *Templates* plugin's template folder at
  [`templates/`](templates/) so new notes start from the right frontmatter. (Keep this the **only** copy.)

## Dashboards: prefer Bases (no code)
Use the **Bases** core plugin for dashboards over your notes' properties. It stores everything in your local
Markdown and a diff-friendly `.base` file, and runs **no arbitrary code**. Useful views:
- **Unverified queue (claim-bearing):** filter `status` is one of `unreviewed`/`proposed`/`reviewed` (status is
  type-scoped — don't just use `status != verified`, since `decision` notes use `accepted`/… and `source`/`index`
  carry no status).
- **Decisions needing attention:** `type` is `decision` and `status` is `proposed`.
- **By domain / type / applies-to version.**
- **Missing provenance:** claim-bearing notes with empty `sources`.
- **Recently updated.**

**Avoid** DataviewJS and Templater *scripts* for shared dashboards — they execute arbitrary JS/shell (a code-execution
risk in a repo meant to be cloned and trusted). Plain Dataview DQL is read-only, but Bases covers the common cases
without a third-party dependency.

## Git hygiene for `.obsidian/`
The provided [`.gitignore`](../.gitignore) excludes the noisy/secret parts:
- `.obsidian/workspace*.json` — per-session UI state (churns constantly).
- `.obsidian/plugins/*/data.json` — plugin settings, which may hold **secrets** (e.g. the Local REST API key).

You may optionally commit curated config (`app.json`, `graph.json`, `core-plugins.json`) so clones share sensible
defaults — but never commit the two patterns above. If in doubt, ignore the whole `.obsidian/` folder (an alternate
line is included, commented, in `.gitignore`).

## Restricted Mode
Obsidian ships with **Restricted Mode on**, which blocks third-party plugin code. Enabling the Local REST API plugin
(for agent access) is a deliberate step — see [`mcp-setup.md`](mcp-setup.md) and [`security.md`](security.md).
