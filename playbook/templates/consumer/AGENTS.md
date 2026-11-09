# AGENTS.md — {{PROJECT}} (pointer; the rules live in the knowledge base)

- **Knowledge base:** `{{KB_NAME}}`, at `${{KB_ENV_VAR}}` (see `.env`) or, when unset, the sibling folder
  `../{{KB_NAME}}`. **Read `playbook/PLAYBOOK.md` there first**; conventions and roles are next to it.
- **Project profile:** `docs/project-profile.md` — read it before anything else; if it is missing, follow the
  knowledge base's `playbook/skills/scope-project/SKILL.md`.
- **Skills:** if your agent does not discover skills on its own, read `playbook/skills/<name>/SKILL.md` in the
  knowledge base whenever the playbook names one.
- **Plans:** `docs/plans/YYYY-MM-DD_<slug>/` (index in `docs/README.md`). Optional checkpoint: `active.md`.
- **Project-specific rules (commands, git policy, secrets):** <add here — keep this file under 10 lines>
