---
name: scope-project
description: Scope a new project or a new feature by asking the right questions first. Use when a user says "build X", "add feature Y", or when a project has no docs/project-profile.md. Produces the profile, ASSUMED defaults, open questions and ADR stubs.
---

# Scope a project or a feature

Domain repos ship a question bank as a note (rationale, options, defaults, blocking flags); this skill is the
procedure. If no such note exists, derive the questions from the categories below and record them as a gap.

1. **Read before asking.** `docs/project-profile.md`, existing ADRs, repo root, README. Never ask what the repo
   already answers. Decide: greenfield, feature on an existing project, or re-scope.
2. **Tier 0 — answers that change the stack or architecture** (platforms, language/stack, who is trusted,
   persistence and accounts, money, licences/attribution/visibility, interfaces, i18n, team and rigor, tooling,
   what counts as v1, audience and data). Pick the ≤ 5 that are both irreversible and unknown; ask them in **one**
   message with 2–4 structured options each and a recommended default. Blocking = `yes` → wait;
   `conditional`/`soft`/`no` → proceed with `ASSUMED:`.
3. **Assume the rest.** Write every unanswered question as `ASSUMED: <question> → <default> (why)` in the plan's
   `open-questions.md` (or in the profile if no plan exists yet).
4. **Tier 1 — design questions** (pitch, audience, scope of v1, non-goals, quality bars, budgets): draft answers
   with defaults; confirm in batches of ≤ 5.
5. **Feature variant.** User problem, ADRs touched, affected modules, data/compatibility, configuration, interface
   per surface, acceptance tests, docs/ADR update, non-goals; default = smallest slice behind a flag.
6. **Write outputs.** `docs/project-profile.md` from `playbook/templates/consumer/project-profile.md` (or the
   domain's profile template); one ADR stub per closed Tier-0 answer (`playbook/templates/adr.md`); re-ask
   triggers noted (new platform, money, more users/players, external service, licence change).
7. **Self-check before presenting** with `playbook/checklists/plan-review.md`.
