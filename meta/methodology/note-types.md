# Note-Type Decision Guide

Eight content types (`type:` frontmatter). This guide says **when to use which**, and resolves the overlaps that
cause confusion. If two types seem to fit, use the disambiguation rules below.

## The eight types at a glance
| type | it is… | atomic? | lives in | has `status`? |
|---|---|---|---|---|
| `source` | a **preserved original** (evidence) | n/a | `sources/` | **no** (provenance only) |
| `concept` | **one idea** explained | lean atomic | `notes/` | yes (claim-bearing) |
| `pattern` | a **reusable solution** to a recurring problem | lean atomic | `notes/` | yes (claim-bearing) |
| `anti-pattern` | a **tempting-but-wrong** approach + the fix | lean atomic | `notes/` | yes (claim-bearing) |
| `reference` | a **coherent whole-surface** page (read as a unit) | whole page | `notes/` | yes (claim-bearing) |
| `guide` | a **how-to / procedure** (ordered steps) | whole page | `notes/` | yes (claim-bearing) |
| `decision` | an **ADR** (a decision + its rationale) | atomic | `notes/` | yes (**decision** ladder) |
| `index` | a **map of content** (navigation) | n/a | `notes/` | **no** (navigational) |

(Status ladders differ by type — see [`lifecycle.md`](lifecycle.md). Claim-bearing =
`unreviewed/proposed/reviewed/verified/deprecated`; `decision` = `proposed/accepted/rejected/superseded`;
`source` and `index` carry no trust status.)

## Disambiguation rules (the overlaps)
**`source` vs `reference` — "did you *preserve* it or *write* it?"**
- `source` = the *original*, preserved verbatim/excerpted, immutable, provenance-stamped. Example:
  `sources/http-caching-rfc.md` — an excerpt of the spec itself.
- `reference` = *your own synthesis* of a surface, citing sources, status-tracked. Example:
  `notes/http-caching.md` — an own-words reference page that cites `sources/http-caching-rfc.md`.

**`concept` vs `reference` — "one idea, or a whole surface?"**
- `concept` = a single idea, lean and atomic. Example: `notes/idempotency.md`.
- `reference` = a coherent surface read as a unit (a whole API, a table, a config set). Example:
  `notes/http-methods.md` (the full method table). If you find a `concept` growing into many sub-sections read
  together, it is becoming a `reference`.

**`pattern` vs `guide` — "a reusable shape, or an ordered procedure?"**
- `pattern` = the reusable solution + when/why + trade-offs (not a step list). Example:
  `notes/retry-with-backoff.md`.
- `guide` = do-this-then-that steps to accomplish a task. Example: `notes/configure-retries.md`.
- A `pattern` may *link to* a `guide` that applies it, and vice versa.

**`index` vs an ordinary note — "does it assert claims, or only organize?"**
- `index` only **links and groups** other notes; it makes **no claims of its own** and carries no trust status.
- If a would-be index starts making claims, split the claims into a `concept`/`reference` and keep the index purely
  navigational.

**`anti-pattern` vs `pattern`** — an `anti-pattern` documents what *not* to do and points (via `contradicts`) to the
correct `pattern`/`concept`. Keep the positive guidance in the `pattern`; keep the warning in the `anti-pattern`.

## Choosing in doubt
1. Preserving an original? → `source`.
2. Pure navigation? → `index`.
3. Recording a decision? → `decision`.
4. Otherwise it's claim-bearing: one idea → `concept`; a whole surface → `reference`; a reusable solution →
   `pattern`; a warning → `anti-pattern`; ordered steps → `guide`.

See [`metadata.md`](metadata.md) for fields and [`../templates/`](../templates/) for a starter per type.
