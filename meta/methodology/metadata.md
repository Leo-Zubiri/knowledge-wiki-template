# Metadata & note conventions

Frontmatter is **portable YAML** — understandable and parseable **without Obsidian**. No field exists without a
defined purpose and a maintenance rule. Domain repos extend the schema **additively**; never impose domain-specific
fields on the core.

## Three independent dimensions
Keep these separate — they answer different questions:

- **Content type** — *what the document is* → `type:`
  `source · concept · pattern · decision · reference · anti-pattern · guide · index`
- **Review status** — *how much it is trusted* → `status:`
  `unreviewed · proposed · reviewed · verified · deprecated` (see [`lifecycle.md`](lifecycle.md))
- **Storage location** — *where it lives* → the **folder** (`inbox/ | sources/ | notes/`).
  This is encoded by the path, **not** a frontmatter field, so it can never disagree with reality.

A preserved **source** therefore has a `type` and lives in `sources/`, but has **no `status`** — it does not ride
the synthesis review lifecycle.

## Fields
| field | applies to | required? | meaning / maintenance rule |
|---|---|---|---|
| `id` | all notes | **yes** | stable, human-readable slug. **Never changes**, even if the title does. |
| `type` | all | **yes** | content type (above). |
| `created` | all | **yes** | date the file was first created. Never edited afterward. |
| `updated` | all | **yes** | date of the last *substantive content* change (not trivial edits). |
| `status` | `notes/` | **yes** (N/A to `sources/`) | review state. Agents set `unreviewed`/`proposed` only. |
| `domain` | `notes/`, `sources/` | **yes** | e.g. `example`. Enables multi-domain use and filtering. |
| `source` | `sources/` | **yes** | the original's provenance: `url`, `title`, `retrieved`, `version` (+ `hash` if useful). |
| `sources` | synthesized `notes/` | **required for `verified`** | list of `{claim, url, retrieved, version}` — the evidence trail. |
| `summary` | `notes/` | recommended | 1–2 line TL;DR for progressive disclosure (lets a reader/agent stop early). |
| `related` | optional | optional | typed, reason-annotated links (see vocabulary below). |
| `author` | all | optional | `human` or `agent` — a *trust* signal (human- vs agent-authored), independent of the Git committer. |
| `applies-to` | `reference`/`concept` | type-scoped | version applicability (e.g. `min`/`tested`) — only where relevant. |
| `superseded-by` | `deprecated` only | type-scoped | `[[successor-note]]` — deprecated knowledge must point to its replacement. |

**Not used:** `last_modified_by` — **Git already records author and full change history**; a frontmatter copy would
be redundant and drift. (`author` is kept only for the human-vs-agent *trust* distinction Git's committer field
doesn't capture.)

### Relationship vocabulary (`related:`)
`relates-to · refines / refined-by · contradicts · supersedes / superseded-by · source-of / derived-from ·
depends-on / prerequisite-of · example-of / has-example`. Always include a short reason for the link.

## Examples
### A synthesized note (`notes/<id>.md`)
```markdown
---
id: event-decoupling
type: concept
domain: example
status: verified
summary: When to prefer event/message decoupling over direct calls, and the coupling trade-off.
sources:
  - claim: "Events decouple the emitter from the receiver"
    url: https://example.com/docs/events
    retrieved: 2026-11-05
    version: "1.0"
related:
  - refines: "[[observer-pattern]]"        # events are a concrete form of the observer pattern
  - contradicts: "[[tight-coupling-antipattern]]"
created: 2026-11-05
updated: 2026-11-05
author: agent
---

## Summary
<1–2 lines, then the body…>
```

### A preserved source (`sources/<id>.md`) — no review lifecycle
```markdown
---
id: events-doc
type: source
domain: example
source:
  url: https://example.com/docs/events
  title: "Events — official documentation"
  retrieved: 2026-11-05
  version: "1.0"
created: 2026-11-05
updated: 2026-11-05
---

> Preserved excerpt or a stable, pinned reference to the original, with enough context to re-derive from it.
```

See [`../templates/`](../templates/) for a ready-to-copy template per `type`.
