# examples/ — a tiny worked example (not knowledge)

Two files you can imitate: one preserved source and one note that cites it.

- [`sources/demo-source-vs-synthesis.md`](sources/demo-source-vs-synthesis.md) — the preserved original.
- [`notes/source-vs-synthesis.md`](notes/source-vs-synthesis.md) — the synthesized note (`status: proposed`) citing it by `ref`.

**Why it lives here and not in `notes/`:** this is demonstration content, not your domain's knowledge. Keeping it outside
`notes/` and `sources/` means retrieval, dashboards and the validation recipe never count it as knowledge. When you copy
a pattern from it, put the real file in `notes/` or `sources/`. **Delete this folder** once you no longer need it.

To check that the example itself is well-formed, run the [validation recipe](../meta/workflows/validation-checklist.md#copy-paste-check-recipe)
from inside this folder (`cd examples`): the two subfolders play the role of `notes/` and `sources/`, and it should print `OK`.
