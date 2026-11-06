# The four methodologies — and who owns what

We combine four established methodologies so each owns **one concern**. We adopt selectively, never mechanically.

| Methodology | Owns here | We do **not** use it for |
|---|---|---|
| **CODE** (Tiago Forte) | the **information lifecycle** (capture → verified knowledge) | folder structure; relationships |
| **PARA** (Tiago Forte) | an **optional task/work layer** only | the reference knowledge core |
| **LLM Wiki** (emerging practice) | **provenance, synthesis discipline, verification** | linking mechanics; lifecycle naming |
| **Zettelkasten** | **reason-annotated, typed linking** (+ atomicity where it helps) | provenance; actionability |

These axes are orthogonal — time/maturity (CODE), truth/provenance (LLM-Wiki), graph/relations (Zettelkasten),
actionability (PARA) — so they layer without conflict.

---

## CODE — the lifecycle
**C**apture → **O**rganize → **D**istill → **E**xpress (Forte,
<https://fortelabs.com/blog/basboverview/>). Capture by *resonance*; distill progressively (raw → highlights →
own-words summary) — "more summarization is not better"
(<https://fortelabs.com/blog/progressive-summarization-a-practical-technique-for-designing-discoverable-notes/>).
Here, CODE becomes **auditable states**, not ad-hoc note stages — see [`lifecycle.md`](lifecycle.md).

## PARA — an optional task layer only
Projects / Areas / Resources / Archive, organized by **actionability** (Forte,
<https://fortelabs.com/blog/para/>). **We do not apply PARA to the knowledge core**: a technical reference has **no
completion state** (everything would collapse to "Resources"), and Archive is an *attention* judgment that would
hide dormant-but-true facts. Instead the core is organized by **domain/topic + review status**.

Keep PARA **only** for optional work *about* the knowledge base — e.g. a migration project or a research push. If
your domain needs this, add top-level `projects/` and `areas/` folders; otherwise leave them out. (This template
does not create them, to avoid empty scaffolding.)

## LLM Wiki — provenance & synthesis discipline
A recent (2026) practice for agent-maintained wikis. The field is young and unproven, so we borrow **shapes, not
code**: a clear split between **preserved raw sources** and **synthesized notes**; **per-claim provenance**;
**synthesize-don't-duplicate**; **human verification** before knowledge is trusted; and lint-style consistency
checks (links resolve, claims cite sources). These map directly onto our [content classes and
states](lifecycle.md).

## Zettelkasten — linking (and selective atomicity)
The highest-value, most transferable principle is **connection over collection**: place each note in relation to
others, and **every link should say *why* it exists** (<https://zettelkasten.de/introduction/>).

On atomicity, the primary source is explicitly **anti-dogmatic**: it calls "one idea per note" *"not really an
explanation"* and defines atomicity as *"separate concerns from one another"* — "a guiding compass rather than a
rigid law" (<https://zettelkasten.de/atomicity/>). So:

- **Adopt (as a universal rule):** reason-annotated, **typed links**; and `index`/MOC notes as cheap entry points.
- **Adapt:** **atomicity is a per-type attribute, not a global rule** — `concept`/`pattern`/`decision` notes lean
  atomic; `reference`/`guide` pages stay whole (separate concerns *within* the page via sections). Use
  **human-readable slug IDs** (not timestamps/Luhmann numbers): better for humans, Git diffs, and path-based
  retrieval. Tags are secondary search phrases, not primary structure.
- **Reject:** *folgezettel* (sequence numbering — overhead that encodes irrelevant authoring order), and "no
  folders" as dogma (we use a light topic structure **plus** links).

### Relationship vocabulary (typed links)
Typed, reason-annotated links live in a note's `related:` frontmatter (and body `[[wikilinks]]` for human/Obsidian
navigation). Each stores **one canonical direction**; inverses are **derived at retrieval**, not hand-maintained.
**Provenance (note→source) is not a relationship** — it lives in the `sources:` field. See the full taxonomy and the
canonical-direction rules in [`relationships.md`](relationships.md).
