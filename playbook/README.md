# playbook/ — letting the knowledge base act on consumer projects (optional layer)

`notes/` is **declarative** knowledge. `playbook/` is a small, capped **procedural** layer so an agent can *use*
that knowledge on any consumer project (an app, a service, a game) with a tiny, committed, agent-agnostic footprint
in that project. Delete this folder and the repository still works exactly as before — it is non-load-bearing, like
the other adapters (see [`../meta/interoperability.md`](../meta/interoperability.md)).

## Caps that keep this from becoming a framework
- **≤ 7 skills, ≤ 4 roles** per domain repo. Add one only when a recorded failure shows it is missing.
- Skills use the open Agent Skills format with standard frontmatter only (`name`, `description`); **no scripts, no
  hooks**, ≤ 3 KB each; they **point to notes and workflows by id/path**, never restate them.
- Nothing here names a specific consumer project. Project facts live in the project (`docs/project-profile.md`).
- The filesystem floor holds: any agent that reads files can follow `PLAYBOOK.md` without skill discovery.

## Layout
| Path | Purpose |
|---|---|
| [`PLAYBOOK.md`](PLAYBOOK.md) | entry point when acting on a project: read its profile → pick a skill → cite notes → write only project facts into the project |
| [`setup.sh`](setup.sh) · [`setup.conf.example`](setup.conf.example) | bootstrap a consumer project once (`setup.sh project <dir>`); `check`, `unlink` |
| [`roles.md`](roles.md) | portable role spec (planner, engineer, qa, librarian) |
| `agents/` | optional Claude Code subagent adapters of the roles (other agents use `roles.md`) |
| `skills/` | generic: `consult-knowledge`, `write-back-knowledge`, `scope-project`, `plan-phase`, `review-acceptance` — a domain repo adds up to two of its own |
| `checklists/` | `plan-review`, `phase-exit` (generic items; domain repos add theirs) |
| `templates/` | consumer contract (`consumer/`), plus `plan`, `phase`, `adr`, `active` |

## The consumer contract (what `setup.sh project` writes; all committed, no absolute paths)
```
AGENTS.md                  ≤ 10 lines: where the knowledge base is, "read playbook/PLAYBOOK.md first", profile path
CLAUDE.md                  @AGENTS.md                       (Claude Code adapter)
.gemini/settings.json      context.fileName includes AGENTS.md (Gemini CLI adapter)
.env.example               <KB_ENV_VAR>=…  (only needed when the base is not the sibling folder ../<kb-name>)
docs/project-profile.md    YAML frontmatter the skills read first (declares plans_dir, active_plan, adr_dir)
docs/README.md · docs/plans/   one dated folder per initiative
.claude/skills/<x>/SKILL.md    optional (--claude): pointer skills so Claude Code / OpenCode trigger them
.agents/skills/<x>/SKILL.md    optional (--codex):  pointer skills for Codex
```
How the base is found, in order: `$<KB_ENV_VAR>` → `.env` → sibling folder `../<kb-name>`.

## Teams and other machines
The bootstrap is run **once by one person** and its output committed. A teammate only clones the project and the
knowledge base (next to each other, or anywhere plus a one-line `.env`). No per-machine install, no symlinks
(they break on Windows and are not portable). `setup.sh check <dir>` tells anyone whether the base is reachable
and the pointer skills are complete. With several contributors, keep the base's default governance (branch + PR,
[`../meta/workflows/git-governance.md`](../meta/workflows/git-governance.md)); a single owner may document an
exception for direct commits on local `main`.

## Which agents get what (honest; from the interoperability matrix)
| Product | Reads the pointer `AGENTS.md` | Triggers skills on its own |
|---|---|---|
| Codex | native | yes, from `.agents/skills` (`--codex`) |
| OpenCode | native | yes, from `.claude/skills` (`--claude`) |
| Claude Code | via `CLAUDE.md` import | yes, from `.claude/skills` (`--claude`) |
| Gemini CLI | via `.gemini/settings.json` | no — follows the playbook's prose |
| Cursor / Copilot | conditional (see matrix) | no — follows the playbook's prose |

## Cross-cutting concerns MOC (recommended for broad domains)
A domain repo whose scope is wide (e.g. game development spanning engine, design, delivery and agent-ops)
benefits from an `index`/MOC note that lists the concerns **every** project in the domain must address at
runtime — error/crash reporting, observability, data integrity, accessibility, i18n — each a one-line pointer
to its note. The `phase-exit` checklist references it so `plan-phase` and `review-acceptance` surface it. This
is the methodology's “structure via MOCs, not deep folders” answer to a growing `notes/`.

## Keeping derived repos in sync
There is no automatic sync between this template and domain repos built from it. A domain repo copies
`playbook/` once, keeps the generic files as they are, adds its own skills/checklists/profile fields, and records
the template version it took them from in its README.
