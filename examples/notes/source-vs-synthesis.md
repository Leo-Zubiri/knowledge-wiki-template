---
id: source-vs-synthesis
type: concept
domain: example
status: proposed
summary: "Keep originals and synthesized notes in separate places so every claim can be traced back and checked."
aliases: [raw vs compiled, evidence vs summary]
sources:
  - claim: "Separating the preserved original from the synthesized note lets a reader verify claims against the original, and corrections do not rewrite the evidence."
    ref: demo-source-vs-synthesis
created: 2026-01-01
updated: 2026-01-01
---

## Summary
Originals live in `sources/`; your own-words restatement lives in `notes/`. The note cites the original, so the claim
can be checked.

## Explanation
The note does not copy the source. It restates the idea and carries a `sources:` entry whose `ref` is the source's `id`.
If the synthesis turns out to be wrong, you fix the note; the preserved source stays untouched. This note is
`status: proposed` — only a human, after the [validation checklist](../../meta/workflows/validation-checklist.md), may
raise it to `verified`.

## See also
- Nothing else to link in this tiny example. In a real repo, add typed `related:` entries (each with a reason) and
  mirror each one here as a wikilink to the target note.
