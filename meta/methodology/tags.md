# Tags — an optional, controlled facet layer

Tags are a **secondary** classification aid. The primary structure stays `type` (what it is), `status` (trust), `domain`
and the folder (location), and the typed `related:` links (see [`metadata.md`](metadata.md),
[`relationships.md`](relationships.md)). A tag adds an **orthogonal facet** that those do not express (e.g. which
department or tool a note concerns) so humans and agents can filter, search and colour the Obsidian graph by it.
**Tags are optional.** Use them only when a facet has proved useful; a repo may ship with an empty vocabulary.

## Rules
- **Controlled vocabulary only.** The allowed tags are the rows of the vocabulary table below. An agent (or human) may use
  *only* listed tags. To introduce a new tag, **add it to the vocabulary in the same diff**; a human approves it in review.
  Free-form, per-ingest invented tags are not allowed: they drift (`godot`/`Godot`/`godot4`), duplicate each other and are
  rarely reviewed.
- **Namespaced, lowercase, kebab-case**: `<facet>/<value>`, e.g. `dept/audio`, `tool/blender`. Keep the number of facets small.
- **At most 4 tags per note.** Tags describe the note's subject, not its type, status or domain (never duplicate those).
- **Only on `notes/`.** Not on `sources/` (immutable evidence; a sources hub index groups them) and not on `inbox/`.
- **Format:** an inline YAML list in the frontmatter, `tags: [dept/audio, tool/ffmpeg]`, so `rg` can match it. Do **not**
  write `#word` in prose — Obsidian turns any `#word` in a note body into a tag.
- **A tag is not a relationship.** Notes sharing a tag are not thereby related; use typed `related:` links and a body
  "See also" for real connections (see [`relationships.md`](relationships.md)).
- **Closed enumerations can be properties instead.** If a facet has a fixed set of values you want to validate and
  filter in Bases, a dedicated frontmatter property can fit better than tags. Tags suit open-ended topics and give
  first-class Obsidian UX (tag pane, autocomplete, graph colour groups).

## Using tags
- **Retrieval:** `rg -l "^tags:.*dept/audio" notes/` — then read the note's `summary`. Tags are a search aid, **not evidence**.
- **Graph:** colour notes by tag with graph **Groups** (`tag:#dept/audio`). Keep the graph's **Tags** toggle **off**: with it on,
  every tag becomes a hub node joining all notes that share it (co-occurrence, not relation).
- **Ingest:** at the *Classify* step, optionally pick tags from the vocabulary (see [`../workflows/intake.md`](../workflows/intake.md)).
- **Validate:** the [check recipe](../workflows/validation-checklist.md#copy-paste-check-recipe) verifies every tag is in the
  vocabulary, at most 4 per note, none on sources, and no stray `#word` in prose.

## Vocabulary (fill per repo)
One row per allowed tag; the first column is the tag in backticks (the recipe reads these rows).

| Tag | Meaning |
|---|---|
| *(none yet — add rows as facets prove useful, e.g. `dept/<name>`, `tool/<name>`)* | |
