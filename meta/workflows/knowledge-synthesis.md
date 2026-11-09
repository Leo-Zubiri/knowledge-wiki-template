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
4. **Cite.** Every non-obvious claim gets a `sources:` entry with a `claim` and **exactly one of `url` (external) or
   `ref` (local source-note id)** — `retrieved` is required with `url`, optional with `ref`. Prefer a local `ref`
   into `sources/` when you've preserved the original (see [source-preservation.md](source-preservation.md) and
   [../methodology/metadata.md](../methodology/metadata.md)).
5. **Relate.** Add typed, reason-annotated `related:` links (and body `[[wikilinks]]` in a "See also" for Obsidian/
   human navigation). **Mirror every `related:` target in the body "See also"** (the graph is built from the body). **Store only the canonical direction** — e.g. `refines`, `depends-on`, `example-of`,
   `contradicts`; the inverse is derived at retrieval, never hand-written (see
   [../methodology/relationships.md](../methodology/relationships.md)). If this conflicts with an existing note, add a
   `contradicts` link and flag it — **do not silently overwrite**.
6. **Tag (optional).** Add up to 4 `tags` from the controlled vocabulary in [../methodology/tags.md](../methodology/tags.md);
   a new tag is proposed in the same diff (human approves). Never on `sources/`.
   **Set status.** Frontmatter `status: proposed`. (Agents may set only the entry states `unreviewed`/`proposed`.)
7. **Validate** before handing off — run the **[validation checklist](validation-checklist.md)** (the single source of
   truth for the checks; not duplicated here).
8. **Hand off for verification.** Commit on a branch and open it for human review
   (see [git-governance.md](git-governance.md)). **Only a human** promotes to `reviewed`/`verified`.

## Promotion to verified (human only)
A reviewer runs the **[validation checklist](validation-checklist.md)**, confirms the claims against the cited sources,
and — if reliable — sets `status: verified`. `reviewed` is the intermediate state when a human has looked but
validation is incomplete. Promotion is a **human/methodological control**, not an automated gate. See the definitions
in [../methodology/lifecycle.md](../methodology/lifecycle.md).
