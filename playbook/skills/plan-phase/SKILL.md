---
name: plan-phase
description: Create or update a dated plan for a project under docs/plans/YYYY-MM-DD_<slug>/ with phases, numbered steps per folder, commands, exit criteria and cited knowledge notes. Use when the user asks for a plan, roadmap or next phase, or when a project profile exists but no active plan.
---

# Plan an initiative in phases

Prerequisite: `docs/project-profile.md` exists (else run `scope-project`).

1. **Folder.** `<plans_dir>/<YYYY-MM-DD>_<slug>/` (creation date, ISO). Copy `playbook/templates/plan.md` →
   `plan.md`; one `phase-NN-<slug>.md` per phase from `playbook/templates/phase.md`. Set `active_plan` in the
   profile and add the row to `docs/README.md`.
2. **Shape.** Phases ordered by *what unblocks the next decision*: foundation → decision spikes (throwaway) →
   core → first end-to-end slice → content → polish → delivery → optional post-v1. Each phase: goal in one sentence,
   roles, notes to consult (ids), numbered steps grouped by folder, commands, exit criteria with evidence,
   knowledge gaps to write back.
3. **Scope discipline.** Build only what the current phase forces; extract shared pieces on their second use.
   ≤ 4 roles. Documents ≤ 150 lines except phase files; each with a `status / updated / owner` header; ADRs
   immutable (supersede).
4. **Decisions.** Every architectural choice → `adr_dir/NNNN-<slug>.md` (`playbook/templates/adr.md`) citing note
   ids; open choices → marked *open until phase X* with the spike that closes them.
5. **Critique.** Run `playbook/checklists/plan-review.md`; add a "Critical review" table (weakness → risk →
   adjustment) to `plan.md` before presenting. Ask only blocking questions.
6. **Close a phase.** `review-acceptance` with `playbook/checklists/phase-exit.md`; update the status table,
   `active.md`, and write back generic findings (`write-back-knowledge`).
