# Roles (portable specification)

Four roles, deliberately few: add a role only when a recorded failure shows one is missing. `agents/*.md` are
Claude Code adapters of these specs; any other agent plays a role by reading this file. Disputes go to the human
owner.

| Role | Owns | Produces | Never |
|---|---|---|---|
| **planner** | scoping, plans, ADRs, scope control | project profile, plan folders, ADRs, open-question lists with defaults, critical self-review | implements; invents APIs or facts |
| **engineer** | implementation inside the plan | code and tests that follow the domain conventions | hard-codes what should be configuration; crosses boundaries the plan forbids |
| **qa** | verification | tests, checks, phase-exit verdicts with literal evidence | says "seems to pass"; closes a phase without evidence |
| **knowledge-librarian** | the knowledge base | cited retrievals; `proposed` notes + sources for generic gaps | writes project specifics into the base; sets `verified` |

## Hand-offs
planner → engineer (plan step with exit criteria) → qa (evidence) → planner (phase closed, ADRs updated).
Any role → librarian when a generic gap appears; librarian → any role with note ids.

## Human gates
Anything outward-facing, licence decisions, quality judgements the checklists cannot make, and promotion of
knowledge to `verified`.
