# sources/ — preserved originals + provenance

The evidence layer. When material is worth keeping, preserve the original (or a stable, pinned reference) here with
full provenance, so any synthesized note can always be traced back and re-derived.

**Rules**
- **Conventionally immutable.** Treat preserved sources as read-only; do not rewrite them. *(This is a convention
  enforced by Git review — not a technical lock. A domain repo may add OS read-only permissions if it wants one.)*
- Every file uses `type: source` and a `source:` block with `url`/`title`/`retrieved`/`version` (and `hash` where
  useful). See [`../meta/methodology/metadata.md`](../meta/methodology/metadata.md).
- Sources do **not** ride the review lifecycle — they carry no `status:`. Their trustworthiness is about *fidelity
  to the original*, recorded by provenance.
- Preserving a source does **not** mean synthesizing it. Synthesis is a separate, deliberate step.

See [`../meta/workflows/source-preservation.md`](../meta/workflows/source-preservation.md).

> This README keeps the folder under version control. Delete it once the folder has real content if you wish.
