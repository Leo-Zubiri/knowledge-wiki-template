# Workflow: Knowledge synthesis

**Use for:** material that deserves to become part of the wiki — a concept, pattern, decision, or reference someone
will want to retrieve later.

**Effort:** deliberate. **Destination:** `notes/`. **Outcome:** a `proposed` note awaiting human verification.

## Steps
1. **Search first (always).** Before writing anything, check whether a note already covers this — see
   [retrieval.md](retrieval.md). For example:
   ```
   rg -l -i "<topic terms>" notes/
   rg -l "^type: concept" notes/ | xargs rg -l -i "<topic>"
   ```
2. **Decide: update or create.**
   - **Update** the existing note if one covers the topic (the default — avoids duplication). Git records the diff.
   - **Create** `notes/<id>.md` only if nothing fits. Copy the matching [template](../templates/).
3. **Synthesize in your own words.** Distill the source(s) into a clear note. Lead with a 1–2 line `summary`. Keep
   `concept`/`pattern`/`decision` notes focused (atomic *where it helps*); keep `reference`/`guide` pages whole,
   separating concerns via sections.
4. **Cite.** Every non-obvious claim gets a `sources:` entry pointing at the preserved source (and/or external URL)
   with `retrieved` date and `version`.
5. **Relate.** Add typed, reason-annotated `related:` links and `[[wikilinks]]`
   (`refines`, `contradicts`, `supersedes`, `depends-on`, …). If this conflicts with an existing note, add a
   `contradicts` link and flag it for review — **do not silently overwrite**.
6. **Set status.** Frontmatter `status: proposed`. (Agents may set only `unreviewed`/`proposed`.)
7. **Validate** before handing off (see the checklist below).
8. **Hand off for verification.** Commit on a branch and open it for human review
   (see [git-governance.md](git-governance.md)). **Only a human** promotes to `reviewed`/`verified`.

## Validation checklist
- [ ] `id`, `type`, `status`, `domain`, `created`, `updated` present and correct.
- [ ] `summary` is a true 1–2 line abstract.
- [ ] Every important claim has a `sources:` citation that resolves.
- [ ] `related:` links point to real notes (`rg -l "^id: <target>" notes/`) and each has a reason.
- [ ] No duplicate: you searched and either updated an existing note or confirmed none fit.
- [ ] Contradictions with existing notes are linked and flagged, not hidden.

## Promotion to verified (human only)
A reviewer confirms the claims against the cited sources and, if reliable, sets `status: verified`. `reviewed` is the
intermediate state when a human has looked but validation is incomplete. See the definitions in
[../methodology/lifecycle.md](../methodology/lifecycle.md).
