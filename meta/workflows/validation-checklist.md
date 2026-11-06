# Validation checklist (for synthesized notes)

A note is promoted to **`verified` only after this checklist passes in human review.** In this phase the checklist is
a **methodological + human-review control, not an automated guarantee** — the checks use **simple, already-available
tools** (`ripgrep`, `ls`, a YAML parse). **No CLI, service, database, or validation engine is introduced.**

Use this for the **knowledge-synthesis** path (see [`knowledge-synthesis.md`](knowledge-synthesis.md)). `source` and
`index` notes are not claim-bearing and do not use the trust ladder.

## Structural checks (deterministic — a reviewer can run these)
- [ ] **Frontmatter parses** as YAML.
- [ ] **Required fields present** for the note's `type` (see [`../methodology/metadata.md`](../methodology/metadata.md)):
      `id`, `type`, `status`, `domain`, `created`, `updated` (claim-bearing); `summary` recommended.
- [ ] **`status` value is valid for the type** (claim-bearing ladder vs `decision` ladder).
- [ ] **Every `sources:` entry** has a `claim` and **exactly one of `url` or `ref`** (never both, never neither).
- [ ] **`retrieved` present on every `url` entry** (optional on `ref` entries).
- [ ] **Each `ref` resolves** — `sources/<id>.md` exists:
      `rg -o 'ref: *\S+' notes/<note>.md` → confirm each id has a file.
- [ ] **Each `url` is syntactically well-formed** (scheme + host). *(Reachability is NOT checked here — see below.)*
- [ ] **`related:` links resolve** (each `[[target]]` has a `notes/<target>.md`), and each carries a **reason**.
- [ ] **Only canonical relations are stored** (no hand-written inverses; see
      [`../methodology/relationships.md`](../methodology/relationships.md)).
- [ ] **No duplicate** — you searched first and either updated an existing note or confirmed none fit.

## Judgment checks (human)
- [ ] The note is **in your own words** (synthesis, not a copy of the source).
- [ ] **Every non-obvious claim is backed** by a `sources:` entry that actually supports it.
- [ ] **Contradictions** with existing notes are linked (`contradicts`) and surfaced, not hidden.
- [ ] The `summary` is a truthful 1–2 line abstract.

## Promotion rule
- All structural checks pass **and** a human has confirmed the judgment checks → a human sets `status: verified`.
- If a human has looked but some evidence/validation is still incomplete → `status: reviewed`.
- Agents never self-promote beyond `proposed`.

## Source liveness (NOT an automatic gate)
A **`verified` note does not lose its status just because an external `url` is temporarily unreachable** (network
error, 5xx, transient outage). Only a **confirmed removal or a materially changed source** (the evidence no longer
supports the claim) triggers **human re-review** → re-source the claim, or move the note to `deprecated`/`superseded`
as appropriate. **Re-evaluate the evidence when there is reason to** (a reported breakage, a periodic audit) — not on
every transient failure, and never automatically. Prefer a local `ref` into `sources/` for claims whose external
originals are volatile.
