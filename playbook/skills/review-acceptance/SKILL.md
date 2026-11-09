---
name: review-acceptance
description: Review a plan, a phase or a deliverable against the playbook checklists and give an evidence-based verdict. Use when the user asks "is this done?", "review this", or before closing a plan phase.
---

# Review against checklists

Checklists: `playbook/checklists/` — `plan-review.md` (before approving a plan), `phase-exit.md` (closing a
phase), plus any domain checklist the knowledge base ships.

1. **Pick the checklist** for the object under review; read the phase's own exit criteria too.
2. **Collect evidence**, never impressions: command + literal output, test report, file list, capture path, note
   ids cited. If evidence is missing, the item is *not verified*, not "probably fine".
3. **Verdict format** (one line per item): `PASS | FAIL | NOT VERIFIED — <evidence or what is missing>`. Then an
   overall verdict: `ACCEPT`, `ACCEPT WITH FOLLOW-UPS` (each with an owner role), or `REJECT` (blocking items
   first).
4. **Human gates.** Mark items only a human can accept (quality judgements, licence decisions, outward-facing
   actions) as `HUMAN GATE` and do not accept them yourself.
5. **Write back.** Tick the phase checklist with the evidence reference; update `active.md`; route generic
   findings to `write-back-knowledge`.
