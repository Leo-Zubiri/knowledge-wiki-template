---
profile_version: 1
project: {{PROJECT}}
knowledge_base: {{KB_NAME}}
stack: []                  # e.g. [php, laravel-11]
platforms: []
team: solo                 # solo | small | team
rigor: medium              # minimal | medium | full
plans_dir: docs/plans
active_plan: ""            # YYYY-MM-DD_slug
adr_dir: ""                # where this project keeps its ADRs
always_notes: []           # knowledge-base note ids every session should load
skip_tags: []              # knowledge-base tags irrelevant to this project
knowledge_gaps: []         # generic topics still missing in the knowledge base
updated: {{DATE}}
---

# Project profile — {{PROJECT}}

Read first by any agent. Keep it short; decisions live in ADRs; procedures live in the knowledge base.

## Pitch
<one sentence>

## Pillars / non-goals
- …

## Routing for agents
| Domain | Load when | Entry note |
|---|---|---|
| <domain> | <task type> | <index note id> |
