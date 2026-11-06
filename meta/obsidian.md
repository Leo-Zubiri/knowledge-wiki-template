# Obsidian configuration guidance (optional, for humans)

Obsidian is the **human** workspace — editing, navigation, Graph, and dashboards. It is **optional**: the repo is a
plain folder of Markdown, so agents and humans can use it with no Obsidian at all. Everything Obsidian computes
(properties, links, backlinks, tags, graph, search) is derived from the files.

## Opening the vault
Open the repository folder in Obsidian as a vault. Obsidian creates a `.obsidian/` config folder on first open —
that's expected. You do **not** need to commit `.obsidian/` for the vault to work.

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
- **Unverified queue:** filter `status != verified`.
- **By domain / type / applies-to version.**
- **Missing provenance:** notes with empty `sources`.
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
