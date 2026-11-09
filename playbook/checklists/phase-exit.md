# Checklist — closing a plan phase

Every item needs evidence (command + literal output, file path, test report, capture). "Seems fine" is NOT VERIFIED.
Domain repos append their own items.

- [ ] Each exit criterion of the phase document has a PASS with an evidence reference.
- [ ] Project check/test/lint targets pass locally and in CI (pinned tool versions).
- [ ] ADRs written for decisions taken; open ADRs listed with their closing phase.
- [ ] Plan status table, `docs/README.md` and `active.md` updated.
- [ ] Generic findings written back to the knowledge base (`proposed`) or listed as `knowledge_gaps` in the profile.
- [ ] Human gates explicitly accepted by the owner.
- [ ] Throwaway code from the phase archived or deleted as the plan said.
- [ ] Cross-cutting runtime concerns (observability, error/crash reporting) addressed for this phase, or explicitly deferred.
- [ ] *(domain repo adds here)*
