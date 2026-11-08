# Validation checklist (for synthesized notes)

A note is promoted to **`verified` only after this checklist passes in human review.** In this phase the checklist is
a **methodological + human-review control, not an automated guarantee** — the checks use **simple, already-available
tools** (`ripgrep`, `ls`, a YAML parse). **No CLI, service, database, or validation engine is introduced.**

Use this for the **knowledge-synthesis** path (see [`knowledge-synthesis.md`](knowledge-synthesis.md)). `source` and
`index` notes are not claim-bearing and do not use the trust ladder.

## Structural checks (deterministic — a reviewer can run these)
- [ ] **Frontmatter parses** as YAML (free-text values quoted — see the [recipe](#copy-paste-check-recipe) below).
- [ ] **Required fields present** for the note's `type` (see [`../methodology/metadata.md`](../methodology/metadata.md)):
      `id`, `type`, `status`, `domain`, `created`, `updated` (claim-bearing); `summary` recommended.
- [ ] **`status` value is valid for the type** (claim-bearing ladder vs `decision` ladder).
- [ ] **Every `sources:` entry** has a `claim` and **exactly one of `url` or `ref`** (never both, never neither).
- [ ] **`retrieved` present on every `url` entry** (optional on `ref` entries).
- [ ] **Each `ref` resolves** — `sources/<id>.md` exists:
      `rg -o 'ref: *\S+' notes/<note>.md` → confirm each id has a file.
- [ ] **Each `url` is syntactically well-formed** (scheme + host). *(Reachability is NOT checked here — see below.)*
- [ ] **`related:` links resolve** (each `[[target]]` has a `notes/<target>.md`), and each carries a **reason**.
- [ ] **Only canonical relations are stored** (no hand-written inverses; see
      [`../methodology/relationships.md`](../methodology/relationships.md)).
- [ ] **No duplicate** — you searched first and either updated an existing note or confirmed none fit.
- [ ] **Every `related:` target also appears in the body "See also"** (the Obsidian graph is built from the body;
      otherwise the typed link exists only for agents).
- [ ] **Sources**: one `url` per source (or an explicit `urls:` list); `capture` is set (`verbatim`/`excerpt`/`paraphrased`);
      every source is cited by at least one note (or deliberately kept uncited).

## Judgment checks (human)
- [ ] The note is **in your own words** (synthesis, not a copy of the source).
- [ ] **Every non-obvious claim is backed** by a `sources:` entry that actually supports it.
- [ ] **Contradictions** with existing notes are linked (`contradicts`) and surfaced, not hidden.
- [ ] The `summary` is a truthful 1–2 line abstract.
- [ ] **Source fidelity:** if a supporting source is `capture: paraphrased`, it was re-fetched or replaced by verbatim/excerpt
      text before promoting the note to `verified`.

## Promotion rule
- All structural checks pass **and** a human has confirmed the judgment checks → a human sets `status: verified`.
- If a human has looked but some evidence/validation is still incomplete → `status: reviewed`.
- Agents never self-promote beyond `proposed`.

## Source liveness (NOT an automatic gate)
A **`verified` note does not lose its status just because an external `url` is temporarily unreachable** (network
error, 5xx, transient outage). Only a **confirmed removal or a materially changed source** (the evidence no longer
supports the claim) triggers **human re-review** → re-source the claim, or move the note to `deprecated`/`superseded`
as appropriate. **Re-evaluate the evidence when there is reason to** (a reported breakage, a periodic audit) — not on
every transient failure, and never automatically. Prefer a local `ref` into `sources/` for claims whose external
originals are volatile.

## Copy-paste check recipe
A documented snippet — **not shipped tooling** (see [AGENTS.md](../../AGENTS.md) → "When the filesystem is not enough").
It exists because a measured failure justified it: in one instance 18 notes carried invalid YAML (unquoted `summary` with
`: `) and neither authors nor reviewers noticed, since "frontmatter parses" had no recipe. Run it from the repo root
(needs `python3` with `PyYAML`; equivalent `rg`/`yq` checks work too). It prints one line per problem and the count.
It checks: YAML parses, `id` = filename, status valid for the type (none for `source`/`index`), each `sources:` entry has exactly
one of `ref`/`url`, each `ref` resolves, body wikilinks resolve, every `related:` target is in the body, one URL per source,
`capture` set, and no source left uncited.

```bash
python3 - <<'PY'
import re, glob, os, yaml
CLAIM = {"unreviewed","proposed","reviewed","verified","deprecated"}
DECISION = {"proposed","accepted","superseded","rejected"}
def load(p):
    t = open(p).read()
    if not t.startswith("---"): return None, t
    _, fm, body = t.split("---", 2)
    return yaml.safe_load(fm), body
notes = {os.path.basename(f)[:-3]: load(f) for f in glob.glob("notes/*.md")}
srcs  = {os.path.basename(f)[:-3]: load(f) for f in glob.glob("sources/*.md")}
notes = {k: v for k, v in notes.items() if v[0]}; srcs = {k: v for k, v in srcs.items() if v[0]}
bad, cited = [], set()
for kind, items in (("note", notes), ("source", srcs)):
    for i, (d, body) in items.items():
        if d.get("id") != i: bad.append(f"{i}: id != filename")
        st, ty = d.get("status"), d.get("type")
        if kind == "source" or ty == "index":
            if st: bad.append(f"{i}: {ty} must not have status")
        elif st not in (DECISION if ty == "decision" else CLAIM): bad.append(f"{i}: invalid status {st}")
        for s in d.get("sources") or []:
            if ("ref" in s) == ("url" in s): bad.append(f"{i}: sources entry needs exactly one of ref/url")
            if "ref" in s:
                cited.add(s["ref"])
                if s["ref"] not in srcs: bad.append(f"{i}: ref {s['ref']} missing")
        links = set(re.findall(r"\[\[([^\]|#]+)", body))
        bad += [f"{i}: broken link {l}" for l in links if l not in notes and l not in srcs]
        for r in d.get("related") or []:
            for v in r.values():
                m = re.search(r"\[\[([^\]]+)\]\]", str(v))
                if m and m.group(1) not in links: bad.append(f"{i}: related {m.group(1)} not in body See also")
        if kind == "source":
            so = d.get("source") or {}
            if " ; " in str(so.get("url", "")): bad.append(f"{i}: several URLs in `url` (use `urls:`)")
            if so.get("capture") not in ("verbatim", "excerpt", "paraphrased"): bad.append(f"{i}: missing/invalid source.capture")
bad += [f"{s}: source never cited" for s in srcs if s not in cited]
print("\n".join(bad) or "OK"); print(len(bad), "problem(s)")
PY
```
