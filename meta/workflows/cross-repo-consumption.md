# Workflow: Cross-repo knowledge consumption

How a **separate project repo** (an app, a service, a game — anything) consults a shared knowledge repo built from this
template, and feeds reusable discoveries back **without embedding it**. This is **filesystem-first** and adds **no new
infrastructure** — it reuses the repo's existing [retrieval](retrieval.md), [intake](intake.md), and
[governance](git-governance.md). It is **domain-agnostic**: examples name Godot, but everything applies to any
`<domain>-knowledge` repo and any consuming project.

> **Golden rules.** The project **points to** the knowledge repo — it never contains a copy. The development context is
> **read-only** to the knowledge repo. Reusable finds are **staged in the project** and turned into notes by a
> **separate, authorized ingestion pass**. Nothing becomes `verified` without a **human** (see
> [`lifecycle.md`](../methodology/lifecycle.md), [`git-governance.md`](git-governance.md)).

## Two repos, two sources of truth
- **`<domain>-knowledge`** (this template, filled in) — the source of truth for **general, reusable, source-grounded**
  knowledge: concepts, patterns, anti-patterns, references, guides, decisions.
- **The project repo** — the source of truth for **that project's own** implementation and decisions (its architecture,
  mechanics, conventions, ADRs, known bugs).

Keep them **independent**. The project *points to* the knowledge repo via a small instruction block + access config —
**not** by vendoring, cloning, or adding it as a **Git submodule** (a submodule embeds a checkout — i.e. a copy — and
muddles whose governance applies). See [`interoperability.md`](../interoperability.md) for why the filesystem is the
portable floor here.

## 1. Reachability & read-only consumption

**Make the knowledge repo reachable** from the project without copying it: point an *additional working directory* at an
independent checkout (in Claude Code, `additionalDirectories` in the project's `.claude/settings.json`, or `--add-dir`;
other agents have equivalents). This references an outside checkout — it is not a copy in the project.

> **Reachable ≠ read-only (enforce the boundary separately).** `additionalDirectories` / `--add-dir` only *add a path to
> the workspace*; the agent then acts there **under the session's existing permission rules**. They do **not** make the
> path read-only. An instruction like "this is read-only" is **agent policy, not a technical control** — exactly the
> distinction [`git-governance.md`](git-governance.md) draws (policy vs. real enforcement). To actually guarantee the
> boundary, enforce it **separately**:
> - a **write-deny permission rule scoped to the knowledge-repo path** (deny `Write`/`Edit`/other write-capable tools on
>   that path in the project's agent permissions), and/or
> - **OS-level isolation** (check out / mount the path read-only for the development context).
>
> Write access to the knowledge repo belongs only to the separate **ingestion pass** (§4) — a different context with
> different permissions.

**Load the knowledge repo's `AGENTS.md` explicitly.** Do **not** assume an `AGENTS.md` living in an *added* directory is
auto-discovered. Agents auto-load instruction files from the **project's own** root only (and in Claude Code a root
`CLAUDE.md` takes precedence over a sibling `AGENTS.md` — see [`interoperability.md`](../interoperability.md)). So the
**project's own** (auto-loaded) `CLAUDE.md`/`AGENTS.md` must **explicitly tell the agent to read
`<knowledge-repo>/AGENTS.md`** before consulting the knowledge base, then follow its [retrieval protocol](retrieval.md):
prefer `verified`, surface `proposed` with its status, **cite note `id`s** in reasoning and in the project's own decision
records, and keep returned context small.

## 2. When knowledge is insufficient → bounded research
Use [`retrieval.md`](retrieval.md)'s **stop rule** to decide whether there is a *real* gap (two passes with different
terms find nothing sufficient → report insufficiency rather than loading more). Only then:
- do **timeboxed** research against **authoritative sources**;
- **never block development** on it — make the best decision you can and record the uncertainty;
- **do not write to the knowledge repo** — stage the find instead (§3).

## 3. Routing — shared vs project-specific (the key guard)
Before preserving anything, decide **where it belongs**. This is the guard against polluting shared knowledge with one
project's choices.

| Ask | If yes → | Goes to |
|---|---|---|
| Would **another** project in this domain benefit from it **unchanged**? | it's general | `<domain>-knowledge` (via staging → ingestion, §4) |
| Is it backed by an **authoritative source**, not just "it worked here"? | it's citable | `<domain>-knowledge` |
| Is it about **this project's** specific choice, mechanic, constraint, convention, or bug? | it's local | the **project repo's** own `docs/`/ADRs — and it stops there |

> Don't confuse one project's working solution with a universal best practice. When in doubt, keep it **local**; a human
> can always promote it later. Project-specific knowledge **never** enters the shared flow.

## 4. Grow the shared knowledge — stage locally, ingest deliberately
The development context is read-only to the knowledge repo, so contributions happen in **two decoupled steps**:

1. **Capture (cheap, in the project).** Stage reusable finds as **raw candidates** in a project-local staging area
   (e.g. `knowledge-outbox/`): the find, its **source/provenance lead**, and a one-line "why it's reusable". This is a
   *delivery mechanism*, not knowledge — it is never cited. Capturing is non-blocking and keeps the dev task moving.
2. **Ingest (deliberate, authorized, separate).** Later, a **human or an explicitly authorized ingestion agent** — a
   context that *does* have write access to the knowledge repo — processes the staged candidates through the repo's
   existing [intake loop](intake.md): **preserve** sources → `sources/`, **search first**, **dedupe**, flag
   **contradictions**, **synthesize** own-words notes → `notes/` as `status: proposed` on a **branch**, then open them
   for **human review** ([`git-governance.md`](git-governance.md), [`validation-checklist.md`](validation-checklist.md)).
   Agents never set `verified`; a human promotes `proposed → reviewed → verified`.

The result is **two independent, separately reviewed diffs**: the project's own PR (implementation + project ADRs), and
later a knowledge-repo PR (`proposed` notes + preserved sources). The next project in the domain benefits.

> **Capture is cheap; synthesis is deliberate.** Not every find earns a note. The staging area may hold candidates that
> are never ingested — a dropped find is a lost *convenience*, never a correctness problem. Make ingestion a routine
> (periodic, or part of PR hygiene) so the backlog stays visible and small.

## 5. Copyable block — drop into the consuming project
Paste this into the **consuming project's own** `AGENTS.md`/`CLAUDE.md` (the file the agent auto-loads), and fill in the
placeholders. Pair it with the access config in the setup note below.

```markdown
## Shared knowledge base (read-only)

This project consults a shared knowledge repository for reusable `<domain>` knowledge. It is an independent repo —
**not** vendored or copied here.

- **Location:** `<path-or-git-url-to-the-knowledge-repo>`
- **Consult before non-trivial `<domain>` decisions:** first **read `<knowledge-repo>/AGENTS.md`**, then follow its
  retrieval protocol to search existing knowledge. Prefer `verified` notes; surface `proposed` ones **with their
  status**; **cite note `id`s** in your reasoning and in this project's decision records (ADRs/`docs/`).
- **Read-only:** never write to the knowledge repo from this project. (This line is policy; the real boundary is enforced
  by permissions/OS — see setup.)
- **If knowledge is insufficient:** do **timeboxed** research against authoritative sources; **do not block development**.
- **Preserve reusable discoveries:** stage them as raw candidates in `knowledge-outbox/` (the find + its source + why
  it's reusable). A **separate, authorized ingestion pass** later turns them into `proposed` notes in the knowledge repo
  for human review. Keep **project-specific** decisions in this project's own `docs/`/ADRs — never in the shared repo.
```

> **Setup note (do this once per project).**
> 1. Make the knowledge repo **reachable**: add its path to the project's `.claude/settings.json` `additionalDirectories`
>    (or launch with `--add-dir`). This points at an independent checkout — not a copy.
> 2. **Enforce read-only separately** (reachability alone does not): add a write-deny permission rule scoped to the
>    knowledge-repo path, and/or check it out read-only at the OS level for the dev context.
> 3. Create the project's `knowledge-outbox/` staging folder.
> 4. Decide **who/when** runs the ingestion pass (a human, or an authorized agent with write access to the knowledge repo).

## Generalizing beyond one domain
Nothing here is Godot-specific. The same pattern lets any project consume any `<domain>-knowledge` repo — point at it,
enforce read-only, load its `AGENTS.md`, route finds with §3, stage and ingest with §4. A project may even consume
**several** knowledge repos (one per domain) by repeating the block per source.

See also: [retrieval](retrieval.md) · [intake](intake.md) · [source-preservation](source-preservation.md) ·
[knowledge-synthesis](knowledge-synthesis.md) · [git-governance](git-governance.md) ·
[validation-checklist](validation-checklist.md) · [`AGENTS.md`](../../AGENTS.md).
