# Checklist — critical review of a plan (run before presenting it)

Answer PASS / FAIL / N/A per item with one line of reasoning; put the failures into a "Critical review" table
(weakness → risk → adjustment) at the top of the plan. Domain repos append their own items below.

## Scope and decisions
- [ ] The ≤ 5 stack-changing questions were asked or explicitly `ASSUMED:` with a default.
- [ ] Each irreversible choice is closed by an ADR or marked *open until phase X* with the spike that closes it.
- [ ] "What counts as v1" is one sentence and the plan stops there; later items are labelled optional.
- [ ] Licence, attribution and cost policies are consistent with reality (mandatory licence texts, fees).

## Over-engineering smells
- [ ] No shared module/framework exists before a second consumer; candidates are marked, not built.
- [ ] ≤ 4 roles; a new role is justified by a recorded failure.
- [ ] Documents: a bounded set, each ≤ 150 lines except phase files; ADRs immutable.
- [ ] No new tool, CLI or orchestration layer without a measured failure of the simpler path.
- [ ] Throwaway prototypes live in a throwaway folder with a closing decision.

## Evidence and honesty
- [ ] Every technical claim cites a note id or an authoritative source; unverified items are marked.
- [ ] Estimates are labelled estimates; nothing is reported as done without evidence.
- [ ] Hidden costs listed (services, fees, signing, devices, people).

## Domain items
- [ ] *(domain repo adds here)*
