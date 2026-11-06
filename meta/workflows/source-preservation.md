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
     url: <original url, or repo@commit, or file reference>
     title: <original title>
     retrieved: <YYYY-MM-DD>
     version: <version/edition if applicable>
     # hash: <optional content hash for integrity>
   created: <YYYY-MM-DD>
   updated: <YYYY-MM-DD>
   ---
   ```
3. Preserve the content:
   - **Text/URL:** paste the relevant excerpt (enough to re-derive from), or the full text if licensing allows.
   - **PDF/binary:** keep the original file alongside, and capture the key extracted text here.
   - **Code repo:** record the **pinned commit** and the specific files/claims you rely on.
4. Record **where** it came from precisely — provenance is the whole point.

## Rules
- Sources are **conventionally immutable**: don't rewrite them later. If the original changes, preserve a **new**
  source (new `retrieved`/`version`) rather than editing the old one.
- Sources carry **no `status:`** — they're evidence, not reviewed knowledge.
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

Next step, only if the material deserves integration → [knowledge synthesis](knowledge-synthesis.md).
