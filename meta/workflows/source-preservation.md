# Workflow: Source preservation

**Use for:** material worth retaining as **evidence** or future reference — documentation, an article, a spec, a
repo at a pinned commit, a PDF's key content.

**Effort:** moderate. **Destination:** `sources/`. **Do NOT synthesize here** — preservation and synthesis are
separate steps.

## Steps
1. Create `sources/<id>.md` with a stable, human-readable `id`.
2. Add provenance frontmatter (copy [`../templates/source.md`](../templates/source.md)):
   ```markdown
   ---
   id: <slug>
   type: source
   domain: <domain>
   source:
     url: "<ONE original url, or repo@commit, or file reference>"
     # urls: ["<more official pages preserved together>"]   # optional
     title: "<original title>"
     retrieved: <YYYY-MM-DD>
     version: "<version/edition if applicable>"
     capture: excerpt          # verbatim | excerpt | paraphrased
     # retrieved_via: "<direct read | API | fetch-summarizer | human copy>"
     # hash: <optional content hash for integrity>
   created: <YYYY-MM-DD>
   updated: <YYYY-MM-DD>
   ---
   ```
3. Preserve the content:
   - **Text/URL:** paste the relevant excerpt (enough to re-derive from), or the full text if licensing allows.
   - **PDF/binary:** keep the original file alongside, and capture the key extracted text here.
   - **Code repo:** record the **pinned commit** and the specific files/claims you rely on.
4. Record **where** it came from precisely — provenance is the whole point — and **how faithfully** (`capture`).
   Tools that fetch a page through a summarizing model return a *paraphrase*, not the original: record
   `capture: paraphrased` (and `retrieved_via`), never `verbatim`.

## Rules
- Sources are **conventionally immutable**: don't rewrite them later. If the original changes, preserve a **new**
  source (new `retrieved`/`version`) rather than editing the old one.
- Sources carry **no `status:`** — they're evidence, not reviewed knowledge.
- **One URL per source** (use the optional `urls:` list only for pages preserved together as one body).
- **Fidelity gates promotion:** a `paraphrased` source may support `proposed` notes; before a note that depends on it is
  promoted to `verified`, re-fetch it or capture `verbatim`/`excerpt` text (a human decision; see the
  [validation checklist](validation-checklist.md)).
- Preserving ≠ trusting a conclusion. Any claim you later make goes in a **note** that *cites* this source.

## How notes cite this source
A synthesized note in `notes/` references this preserved source **by its `id`** through a `sources:` entry — it does
**not** re-paste the content:
```yaml
sources:
  - claim: "<the claim this source supports>"
    ref: <this-source-id>     # resolves to sources/<this-source-id>.md
    # retrieved: optional here — this source note already records its own `retrieved`
```
Because `ref` is the stable id (= filename), keep the filename immutable: if the original changes, preserve a **new**
source note rather than renaming this one.

**Finding what cites a source (source backlinks):** `ref` is a plain id, **not** a `[[wikilink]]` — so a source does
**not** show up in wikilink backlinks or Obsidian's graph, and a well-cited source still looks like a graph **"orphan"
by design**. This is expected, not a broken link. To list the notes that cite a source, grep its id:
`rg -l "ref: <this-source-id>" notes/`.

**Making sources visible to humans (optional hub):** the plain-id `ref` is the *machine layer* (deterministic checks,
immutable sources, no backlink maintenance). Its cost is the human layer: Obsidian's graph and backlinks pane do not
show which notes cite a source, and a reviewer has to open each source by hand. When a domain has more than a handful of
sources, add one **sources hub** — an `index` note (no status, no claims) that links every source with `[[src-…]]`,
grouped by theme, plus the notes that cite it (find them with the `rg` above). It connects the sources to the graph through
one navigational node, changes no citation and no source file, and is updated when a source is added. (Alternative: a
`[[source]]` link in each note's body — direct edges, but duplicated in two places and prone to drift.) The check
"every source is cited by at least one note" is in the [validation checklist](validation-checklist.md).

Next step, only if the material deserves integration → [knowledge synthesis](knowledge-synthesis.md).
