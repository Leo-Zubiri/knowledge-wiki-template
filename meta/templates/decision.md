---
id: <slug>
type: decision
domain: <domain>
status: proposed          # decision ladder: proposed → accepted → superseded (or → rejected)
summary: <one-line statement of the decision made>
sources:
  - claim: "<evidence informing the decision>"
    # provide EXACTLY ONE of url (external) or ref (local source-note id):
    url: <https://external-source>
    # ref: <local-source-id>              # → sources/<local-source-id>.md
    retrieved: <YYYY-MM-DD>               # required with url; optional with ref
related:
  - relates-to: "[[<related-note-id>]]"   # <reason>
  # When THIS decision is later replaced, set `status: superseded` and add:
  # - superseded-by: "[[<newer-decision-id>]]"
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
# created_by_type: agent                  # optional actor-provenance (human|agent); omit if unsure — NOT a trust signal
---

## Context
<The situation and forces that required a decision.>

## Decision
<What was decided, stated plainly. `accepted` means adopted — not that every supporting claim is independently verified.>

## Consequences
<Results, trade-offs, follow-ups. If later replaced, set `status: superseded` and add a `superseded-by` link.>

## See also
- [[<related-note-id>]] — <reason>
