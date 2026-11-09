# Metadata & note conventions

Frontmatter is **portable YAML** — understandable and parseable **without Obsidian**. No field exists without a
defined purpose and a maintenance rule. Domain repos extend the schema **additively**; never impose domain-specific
fields on the core.

## Three independent dimensions
Keep these separate — they answer different questions:

- **Content type** — *what the document is* → `type:`
  `source · concept · pattern · decision · reference · anti-pattern · guide · index`
  (when to use which: [`note-types.md`](note-types.md)).
- **Review status** — *how much it is trusted* → `status:` — **scoped by type** (see [`lifecycle.md`](lifecycle.md)):
  - claim-bearing (`concept/pattern/reference/anti-pattern/guide`): `unreviewed · proposed · reviewed · verified · deprecated`
  - `decision`: `proposed · accepted · rejected · superseded`
  - `source` and `index`: **no trust status**.
- **Storage location** — *where it lives* → the **folder** (`inbox/ | sources/ | notes/`).
  Encoded by the path, **not** a frontmatter field, so it can never disagree with reality.

## Fields
| field | applies to | required? | meaning / maintenance rule |
|---|---|---|---|
| `id` | all notes | **yes** | stable, human-readable slug = the filename. **Never changes**, even if the title does. |
| `type` | all | **yes** | content type (above). |
| `created` | all | **yes** | date the file was first created. Never edited afterward. |
| `updated` | all | **yes** | date of the last *substantive content* change (not trivial edits). |
| `status` | claim-bearing + `decision` | **yes** (N/A to `source`/`index`) | review state, per the type's ladder. Agents set `unreviewed`/`proposed` only. |
| `domain` | `notes/`, `sources/` | **yes** | e.g. `example`. Enables multi-domain use and filtering. |
| `source` | `sources/` | **yes** | the original's provenance: one `url` (optional `urls:` list), `title`, `retrieved`, `version`, **`capture`** (`verbatim`/`excerpt`/`paraphrased`; recommended) (+ `retrieved_via`, `hash` if useful). |
| `sources` | synthesized `notes/` | **required for `verified`** | list of provenance entries (see rules below). |
| `summary` | `notes/` | **expected** | 1–2 line TL;DR for progressive disclosure — agents read it *before* the body, so it drives retrieval. Strongly recommended on every note. |
| `aliases` | `notes/` | optional | alternative names / synonyms for the note's topic. **Improves lexical recall** (finding a concept by a different term) and is Obsidian-native. e.g. `aliases: [signal, event emitter]`. |
| `tags` | `notes/` | optional | controlled facet tags from the vocabulary in [`tags.md`](tags.md) (≤4, inline list, e.g. `tags: [dept/audio]`); a secondary search/graph aid, never a substitute for `type`/`domain`/`status` or typed links. Not on `sources/`. |
| `related` | optional | optional | typed, reason-annotated links — see [`relationships.md`](relationships.md). |
| `created_by_type` | all | optional | `human` or `agent` — **actor-provenance only, NOT a trust signal.** No default; never fabricate. |
| `applies-to` | `reference`/`concept` | type-scoped | version applicability (e.g. `min`/`tested`) — only where relevant. |
| `superseded-by` | deprecated/superseded notes | type-scoped | `[[successor-id]]` — set **only when a specific replacement exists** (see lifecycle). |

**Notes on fields:**
- **Quote free-text strings in YAML** (`summary`, `title`, `url`, `claim`). An unquoted value containing `: ` (colon +
  space) or starting with a special character is invalid YAML and silently breaks metadata tools; the templates ship
  quoted placeholders for that reason.
- **`capture`** says how faithfully a source body reproduces the original. A `paraphrased` source (e.g. produced by a
  summarizing fetch tool) is second-hand evidence and should be re-fetched before dependent notes are `verified`.
- **`created_by_type` is provenance, not trust.** Trust comes from `status` + `sources`/evidence + human review —
  never from who authored a note. Templates ship **no default** for it (no misleading attribution): an agent adds
  `created_by_type: agent` when it authors; a human may add `human`; otherwise omit it (Git records identity/history).
- **No `last_modified_by`** — Git (`git log`/`git blame`) already records who changed what, when.

## `sources:` — provenance rules (claim-level)
Each entry records one claim and where it comes from:
- **Required:** `claim`, and **exactly one of `url` (external) or `ref` (local)** — never both, never neither.
- **`retrieved`:** **required with `url`** (external evidence is time-sensitive); **optional with `ref`** (the local
  source note already records its own `retrieved`). `version` optional in both.
- **`ref` is a plain stable `id`** (= the `sources/` note's filename), resolving to `sources/<id>.md`. It is **not** a
  `[[wikilink]]`: Obsidian does not treat links inside nested frontmatter as real links, and a plain id is honest,
  trivially checkable, and stable against title changes (links resolve by filename). **ids/filenames are immutable** —
  to update an original, preserve a *new* source note, don't rename the old one.
- **Checks:** a `ref` must resolve to an existing file (deterministic); a `url` must be syntactically valid.
  **URL reachability is not a gate** — a temporarily unreachable URL does not demote a `verified` note (see the
  [validation checklist](../workflows/validation-checklist.md) → source liveness).

```markdown
sources:
  - claim: "Events decouple the emitter from the receiver"
    ref: events-doc                # local preserved source → sources/events-doc.md
    retrieved: 2026-11-05
  - claim: "Measured ~40% latency reduction under load"
    url: https://example.com/benchmark
    retrieved: 2026-11-05
    version: "1.0"
```

## Relationships
Typed, reason-annotated links live in `related:`; **provenance does not** (that's `sources:`). Store the canonical
direction only; inverses are derived at retrieval. Full taxonomy: [`relationships.md`](relationships.md).

## Examples
### A synthesized note (`notes/<id>.md`)
```markdown
---
id: event-decoupling
type: concept
domain: example
status: verified
summary: "When to prefer event/message decoupling over direct calls, and the coupling trade-off."  # quoted: safe if it ever contains ': '
sources:
  - claim: "Events decouple the emitter from the receiver"
    ref: events-doc
    retrieved: 2026-11-05
related:
  - refines: "[[observer-pattern]]"        # events are a concrete form of the observer pattern
  - contradicts: "[[tight-coupling]]"      # the opposite coupling choice
created: 2026-11-05
updated: 2026-11-05
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
  capture: excerpt          # verbatim | excerpt | paraphrased
created: 2026-11-05
updated: 2026-11-05
---

> Preserved excerpt or a stable, pinned reference to the original, with enough context to re-derive from it.
```

See [`../templates/`](../templates/) for a ready-to-copy template per `type`.
