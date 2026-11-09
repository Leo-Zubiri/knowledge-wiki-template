# Playbook — how an agent acts on a consumer project using this knowledge base

Applies when you work inside a project whose `AGENTS.md` points here. Everything generic lives in this repository
(`notes/`, `playbook/`); the project keeps only its own facts, decisions and plans. Never write project specifics
into the knowledge base. *(Domain repos: extend the marked sections; keep the structure.)*

## 1. Start of session
1. Read the project's `AGENTS.md` and `docs/project-profile.md` (YAML). No profile → run skill `scope-project`
   before anything else.
2. Read `active.md` if present (checkpoint) and the active plan's current phase (`<plans_dir>/<active_plan>/`).
3. Load knowledge by profile: open the index notes the profile names (`always_notes`); read `summary` and
   frontmatter first; full body only for the 1–3 notes the task needs (skill `consult-knowledge`).

## 2. Pick the procedure
| User intent | Skill | Output in the project |
|---|---|---|
| "build X", "add feature Y", "re-scope" | `scope-project` | `docs/project-profile.md`, open questions with `ASSUMED:` defaults, ADR stubs |
| "plan", "roadmap", "what next" | `plan-phase` | `docs/plans/<date>_<slug>/plan.md` + `phase-NN.md` from templates |
| before any non-trivial domain decision | `consult-knowledge` | cited note ids |
| a generic knowledge gap appears | `write-back-knowledge` | `sources/` + `notes/` (`proposed`) in this repository |
| "review", "is this done?" | `review-acceptance` | verdict against `checklists/` |
| *<domain>: add domain skills here* | | |

If your agent does not discover skills on its own, read `playbook/skills/<name>/SKILL.md` here when this table
names one.

## 3. Rules while working (details: `conventions.md` in domain repos)
- Cite note ids in reasoning, ADRs and commit messages; prefer `verified` > `reviewed` > `proposed`; say the status.
- Questions: ask the ≤ 5 highest-leverage ones with structured options and a recommended default; assume the rest
  and list them as `ASSUMED:`; never block on a question you can default safely.
- Before presenting a plan, run `checklists/plan-review.md` and show the critique.
- Git: follow the project's `AGENTS.md` policy; small commits; never force-push.

## 4. Knowledge base access
- **Read:** filesystem-first (`rg` over `notes/`, then read 1–3 notes) — `meta/workflows/retrieval.md`. If an
  Obsidian MCP is configured, its capped search is an accelerator; never use its write tools.
- **Write:** generic gaps go through `meta/workflows/intake.md` (`sources/` + `notes/`, `status: proposed`, or
  `unreviewed` + evidence class *interpretation* for own patterns). Default governance: branch + PR, or stage in the
  project's `knowledge-outbox/` (`meta/workflows/cross-repo-consumption.md`). A domain repo may document an owner
  exception (direct commits on local `main`). Never `verified`. Never `push` unless told. `grep -ri "<project
  name>"` over the new files must return nothing.
- If the knowledge base is unreachable: say so, do bounded research, continue.

## 5. End of session
Update `active.md` (≤ 25 lines), tick the phase checklist, record decisions as ADRs (`templates/adr.md`), and
list any generic finding not yet written back.
