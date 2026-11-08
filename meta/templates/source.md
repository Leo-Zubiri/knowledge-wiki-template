---
id: <slug>
type: source
domain: <domain>
source:
  url: "<ONE original URL, or repo@commit, or file reference>"
  # urls:                          # optional: several official pages preserved together (then `url` is the main one)
  #   - "<https://…>"
  title: "<original title>"
  retrieved: <YYYY-MM-DD>
  version: "<version/edition if applicable>"
  capture: excerpt                 # verbatim | excerpt | paraphrased — how faithfully the body reproduces the original
  # retrieved_via: "<how it was obtained: direct read, API, fetch-summarizer, human copy>"
  # hash: <optional content hash for integrity>
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
---

> Preserved original (conventionally immutable). Paste the relevant excerpt — enough to re-derive conclusions from —
> or a stable, pinned reference to the full original. Do NOT synthesize here; synthesis goes in `notes/` and cites
> this source by its `id`.
>
> **Set `capture` honestly:** `verbatim` (full text), `excerpt` (exact quoted passages), or `paraphrased` (a summary
> of the original made by a person, a tool or a model — e.g. output of a fetch-time summarizer). A `paraphrased`
> source is second-hand evidence: it may support `proposed` notes, but re-fetch it (or capture verbatim text) before a
> note that depends on it is promoted to `verified`.

<preserved content / excerpt>
