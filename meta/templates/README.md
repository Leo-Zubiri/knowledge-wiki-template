# meta/templates/ — note templates (one per content type)

Copy the template that matches the note's **content type**, rename it to `<id>.md` in the right folder
(`notes/` for everything except `source`, which goes in `sources/`), and fill it in. See
[`../methodology/metadata.md`](../methodology/metadata.md) for field rules.

These are the **single** copy of the templates — they double as Obsidian templates (point Obsidian's *Templates*
core plugin at this folder; see [`../obsidian.md`](../obsidian.md)). Do not keep a second copy elsewhere.

| Template | `type` | Atomic? | Goes in |
|---|---|---|---|
| [`source.md`](source.md) | `source` | n/a | `sources/` |
| [`concept.md`](concept.md) | `concept` | lean atomic | `notes/` |
| [`pattern.md`](pattern.md) | `pattern` | lean atomic | `notes/` |
| [`decision.md`](decision.md) | `decision` | atomic (ADR) | `notes/` |
| [`reference.md`](reference.md) | `reference` | whole page | `notes/` |
| [`anti-pattern.md`](anti-pattern.md) | `anti-pattern` | lean atomic | `notes/` |
| [`guide.md`](guide.md) | `guide` | whole page | `notes/` |
| [`index.md`](index.md) | `index` | n/a (MOC) | `notes/` |

Dates use `YYYY-MM-DD`. Agents may set `status:` to `unreviewed`/`proposed` only.
