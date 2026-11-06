---
id: <slug>
type: pattern
domain: <domain>
status: proposed
summary: <one-line statement of the problem this pattern solves>
# aliases: [synonym, alt-term]           # optional — improves lexical recall
sources:
  - claim: "<claim / where this pattern is described>"
    # provide EXACTLY ONE of url (external) or ref (local source-note id):
    url: <https://external-source>
    # ref: <local-source-id>              # → sources/<local-source-id>.md
    retrieved: <YYYY-MM-DD>               # required with url; optional with ref
    version: "<version if relevant>"
related:
  - refines: "[[<concept-note-id>]]"      # <this pattern is a concrete form of…>
  - contradicts: "[[<anti-pattern-id>]]"  # <the pitfall it avoids>
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
# created_by_type: agent                  # optional actor-provenance (human|agent); omit if unsure — NOT a trust signal
---

## Problem
<What recurring situation does this address?>

## Solution
<The pattern, in your own words. Focused and reusable.>

## Trade-offs
<When to use it, when not; costs.>

## See also
- [[<related-note-id>]] — <reason>
