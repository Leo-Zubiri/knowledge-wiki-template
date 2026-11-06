# Workflow: Quick capture

**Use for:** a link, a short idea, an observation, a temporary reference — anything worth not-forgetting but not yet
worth processing.

**Effort:** minimal. **Destination:** `inbox/`.

## Steps
1. Create a file in `inbox/` (any readable name, e.g. `inbox/2026-11-05-some-idea.md`).
2. Paste or write the thing. One capture per file, or a running daily note — either is fine.
3. Frontmatter is **optional**. At most:
   ```yaml
   ---
   type: source        # optional hint; omit if unsure
   created: 2026-11-05
   ---
   ```
4. Stop. **You are done.** A capture does **not** have to become a note.

## Rules
- Capture by **resonance** — if it's interesting or useful, grab it; don't polish.
- Items in `inbox/` are **never cited** as knowledge and **may stay here indefinitely**.
- If/when an item earns more, escalate:
  - worth keeping as **evidence** → [source preservation](source-preservation.md)
  - worth **integrating** into the wiki → [knowledge synthesis](knowledge-synthesis.md)

## When an input should NOT become a wiki note
- It's ephemeral or low-value → leave it in `inbox/` (or delete it).
- It **duplicates** an existing note → update that note instead (see synthesis).
- It's raw **evidence** → it belongs in `sources/`, not `notes/`.
- It's an uncorroborated passing observation → keep it `inbox/` as `unreviewed`.

> Default bias: **capture is cheap; synthesis is deliberate.**
