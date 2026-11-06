# dashboards/ — ready-made Bases views (optional, for humans in Obsidian)

Pre-made **Bases** dashboards over your notes' frontmatter. They need the **Bases** core plugin enabled (it is, via
`.obsidian/core-plugins.json`). All data stays in your Markdown — these are just views.

> **Honest caveat:** the Bases `.base` YAML schema is **versioned by Obsidian and not a stable public API**. The `.base`
> files here are **best-effort** and may not render on every Obsidian version. If one doesn't show, recreate it in 30s
> via the Bases UI (New base → add the filter shown below). They are **optional** — the repo works without them.

## The dashboards (name · purpose · filter)
| File | Shows | Filter (recreate via UI if needed) |
|---|---|---|
| `unverified-queue.base` | claim-bearing notes not yet verified | `status` is one of `unreviewed`/`proposed`/`reviewed` and `file.inFolder("notes")` |
| `missing-provenance.base` | notes with no `sources` | `file.inFolder("notes")` and `sources` is empty |

Useful extra views to add yourself (via UI): **by `domain`/`type`**, **recently `updated`**, **decisions in `proposed`**.
Status is **type-scoped** (claim-bearing vs `decision`; `source`/`index` have none) — don't just filter `status != verified`
(see [`../meta/methodology/lifecycle.md`](../meta/methodology/lifecycle.md)).

## How to create a Base manually (if a file doesn't render)
1. In Obsidian: command palette → **"Bases: Create new base"** (or insert a `base` code block in a note).
2. Add a **filter** from the table above and choose a **Table** view.
3. Add columns: `file.name`, `status`, `domain`, `updated`.
