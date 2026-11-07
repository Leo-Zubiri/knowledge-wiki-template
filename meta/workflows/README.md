# meta/workflows/ — how work actually gets done

Operational procedures for humans and agents. All of them are **filesystem-first**: they work with **Obsidian
closed** and **MCP unavailable**.

**Ingestion is proportional** — match the effort to the value of the input. Most captures never become wiki notes.

- [`intake.md`](intake.md) — **the structured feeding loop** that ties the three paths below together (triage →
  preserve → synthesize); route-by-input-type table; how to ask an agent to ingest.
- [`quick-capture.md`](quick-capture.md) — a link/idea/observation → `inbox/`. Minimal effort; may stay forever.
- [`source-preservation.md`](source-preservation.md) — keep an original as evidence → `sources/`. No synthesis.
- [`knowledge-synthesis.md`](knowledge-synthesis.md) — integrate material into the wiki → `notes/`. Search first,
  update-or-create, cite, link, propose for human verification.
- [`retrieval.md`](retrieval.md) — find and read knowledge with ripgrep + Git (no Obsidian/MCP needed).
- [`cross-repo-consumption.md`](cross-repo-consumption.md) — **consume this repo from a separate project** (read-only
  consult + stage reusable finds back via a deliberate ingestion pass). Domain-agnostic; no new infra.
- [`validation-checklist.md`](validation-checklist.md) — the checks a human runs before promoting a note to `verified`.
- [`git-governance.md`](git-governance.md) — branches, review, who may change `status`, and what Git actually enforces.

If you are an agent, start from [`../../AGENTS.md`](../../AGENTS.md).
