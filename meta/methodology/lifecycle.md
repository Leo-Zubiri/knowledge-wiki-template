# The knowledge lifecycle — content classes, states, and governance

This is the heart of the method: how raw input becomes trusted knowledge **without an observation ever silently
becoming "verified".**

## Three content classes (physically separated, differently trusted)
Keeping these in separate folders means a wrong synthesis can always be re-derived from a preserved source.

| Class | Folder | Trust | Mutability |
|---|---|---|---|
| **Capture** | `inbox/` | untrusted, never cited | mutable |
| **Preserved source** | `sources/` | evidence (fidelity to original) | conventionally immutable |
| **Synthesized note** | `notes/` | knowledge (grows by review) | edited by consolidation |

## Review states — scoped by note type
Status is **not** one-size-fits-all. `source` and `index` notes carry **no trust status** (a source is preserved
evidence; an index is navigation). The two ladders:

**Claim-bearing notes** (`concept/pattern/reference/anti-pattern/guide`):
```
unreviewed → proposed → reviewed → verified → deprecated
```
- **unreviewed** — captured/observed; not yet put forward. Default for agent observations.
- **proposed** — an agent or human has synthesized it and put it forward for review.
- **reviewed** — a human has examined the note and its claims, but some evidence or validation may remain incomplete.
- **verified** — a human has confirmed the relevant claims against traceable sources and approved the note as
  reliable knowledge. Requires passing the [validation checklist](../workflows/validation-checklist.md) (≥1 resolvable
  source).
- **deprecated** — no longer true or relevant. **A deprecated note need NOT have a successor.** If a *specific*
  replacement exists, point to it with the **`superseded-by`** relationship (see
  [`relationships.md`](relationships.md)) — otherwise just `deprecated`.

**Decision notes** (`decision`, ADR semantics):
```
proposed → accepted → superseded      (or proposed → rejected)
```
- **proposed** — a decision put forward. **accepted** — adopted. **`accepted` does NOT mean every underlying claim is
  verified**; it means the decision was made. **rejected** — considered and turned down. **superseded** — replaced by
  a newer decision; point to it with `superseded-by`.

**Who changes status:** agents may set `unreviewed` / `proposed` only. **Only a human** sets `reviewed` / `verified` /
`deprecated` (claim-bearing) and `accepted` / `rejected` / `superseded` (decision), recorded through Git review (see
[`../workflows/git-governance.md`](../workflows/git-governance.md)).

## The governance rule (non-negotiable)
Agents may **capture, analyze, synthesize, and propose** — but an **agent observation never auto-becomes verified
knowledge.** Always preserve the distinction between:

**original source · unprocessed input · agent observation · proposed synthesis · reviewed knowledge · verified knowledge**

## The lifecycle as nine stages (a conceptual frame)
The operational path is **proportional** (see the three workflows) — most captures never reach stage 4+. The full
frame, for reference:

1. **Capture** — input enters `inbox/` (human or agent).
2. **Ingest** — identify the input type; preserve the original into `sources/` with provenance.
3. **Classify** — assign `domain`/`type`; **search existing notes first** to decide update-vs-create.
4. **Distill / synthesize** — write an own-words note in `notes/`, citing sources. → `proposed`.
5. **Relate** — add typed `related:` links and `[[wikilinks]]`.
6. **Validate** — provenance present, links resolve, claims cite sources, contradictions flagged.
7. **Publish** — a human promotes to `reviewed` → `verified`.
8. **Retrieve** — humans/agents query and read progressively (see [retrieval](../workflows/retrieval.md)).
9. **Maintain** — detect obsolescence; **update** existing notes (Git keeps history); `deprecate` when outdated (link a
   successor via `superseded-by` only if a specific replacement exists).

## Contradiction & obsolescence (handle, don't pretend to automate)
When new material conflicts with an existing note, **do not silently overwrite**. Record it explicitly: add a
`contradicts` link, or a short `type: anti-pattern`/divergence note, and raise it for human review. Mark outdated
knowledge `deprecated` (and add `superseded-by` only when a specific replacement exists). *(No studied tool reliably
automates contradiction detection; here it
is an agent **workflow** plus human judgment, not a feature claim.)*
