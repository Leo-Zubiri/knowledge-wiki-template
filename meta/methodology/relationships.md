# Relationship Taxonomy

Notes connect to each other through **typed, reason-annotated links** in the `related:` frontmatter (and via body
`[[wikilinks]]`). This is the Zettelkasten core: *connection over collection* — **every link should say why it
exists.**

> **Provenance is NOT here.** A note's link to its *sources* lives in the `sources:` field (see
> [`metadata.md`](metadata.md)), not in `related:`. That is why there is no `source-of`/`derived-from` relation.

## Canonical directions — store one end, derive the other
To avoid drift, **each relation has ONE canonical direction that you store.** The inverse is **derived at retrieval**
(a backlink search), never written by hand.

| Store (canonical) | Meaning | Inverse (derived, do NOT store) |
|---|---|---|
| `refines` | A is a more specific/improved treatment of B | `refined-by` |
| `depends-on` | A requires B first | `prerequisite-of` |
| `example-of` | A is a concrete example of B | `has-example` |
| `superseded-by` | A is replaced by B (put on the **older** note) | `supersedes` |
| `contradicts` | A conflicts with B | *(symmetric — store once, on either note)* |
| `relates-to` | generic, non-directional association | *(symmetric)* |

### How to derive an inverse (no stored reciprocals)
To find what refines note `X`, what depends on `X`, what `X` is an example for, or what `X` supersedes, **search for
the canonical link pointing at `X`:**
```sh
rg -n "refines: .*\bX\b|depends-on: .*\bX\b|example-of: .*\bX\b|superseded-by: .*\bX\b" notes/
```
or, more simply, find every note that links to `X` at all (backlinks):
```sh
rg -l "\[\[X\]\]|\bX\b" notes/
```
(See [`../workflows/retrieval.md`](../workflows/retrieval.md) → "Deriving inverse relationships.")

## Writing a relation
In `related:`, each entry is a single-key map `type: "[[target-id]]"` with a trailing comment stating the reason:
```yaml
related:
  - refines: "[[observer-pattern]]"        # events are a concrete form of the observer pattern
  - contradicts: "[[tight-coupling]]"      # this decoupling is the opposite choice
  - depends-on: "[[event-loop]]"           # needs an event loop to deliver events
```
Rules:
- Use the **canonical** relation; never also add its inverse on the other note.
- Always include the **reason** (the comment) — an unexplained link creates no knowledge.
- Use **`superseded-by` on the older/deprecated note** to point at its replacement (the lifecycle link); see
  [`lifecycle.md`](lifecycle.md).
- `relates-to` is the generic fallback — prefer a specific relation when one fits.

## Obsidian note
Because `related:` is a nested frontmatter structure, Obsidian does **not** render it as clickable Property links or
graph edges (nested frontmatter is unsupported there). That is fine: `related:` is **machine/agent metadata**
(greppable). For **human/Obsidian graph navigation**, the note body's **"See also" `[[wikilinks]]`** are what Obsidian
indexes. The two serve different layers; keep typed relations in `related:` and a light human-facing set in the body.

## Unlinked Mentions — a discovery aid, not a link
Obsidian's **Unlinked Mentions** panel (and Search) surfaces notes where another note's **name or alias** appears in the
body **without** a link. Use it during **synthesis and relationship review** to *spot candidate* relationships you might
have missed — it is a **human discovery aid, not a source of truth**.

- **A mention is not a relationship.** Do **not** auto-convert unlinked mentions into `[[wikilinks]]` or "link all" in
  bulk. Treating co-occurrence as connection produces noise and graph-cosmetic links — the opposite of *connection over
  collection*.
- **Promote only real, typed relationships.** When a mention reflects a genuine relation, record it as a **canonical,
  reason-annotated `related:`** entry (above); if it is provenance, record it in `sources:` (`ref`/`url`) instead. Those
  greppable fields — not the mention — are the portable, canonical record.
- Obsidian is only the human lens here; **typed `related:` links and the backlink/`ref:` greps remain the
  agent/CI source of truth.**
