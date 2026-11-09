# Quickstart — from zero to a working knowledge flow

End-to-end setup for a knowledge repo made from this template. Everything except step 1 is optional layering; the repo
is fully usable with just **Markdown + Git**.

> **Shortcut:** ask your agent **"help me set up this knowledge repo"** — it runs the file-based steps and guides the
> rest. Full checklist: [`SETUP.md`](SETUP.md).

## 1. Create your domain repo
```bash
git clone git@github.com:Leo-Zubiri/knowledge-wiki-template.git <domain>-knowledge
cd <domain>-knowledge
rm -rf .git && git init        # fresh history for your domain (or keep it)
```
Pick your `domain:` value (e.g. `laravel`) — you'll put it in each note's frontmatter. Read
[`README.md`](README.md) for the architecture and [`AGENTS.md`](AGENTS.md) if an agent will work here.

## 2. (Optional) Open in Obsidian — for humans
Open the folder as a vault. Point the *Templates* core plugin at [`meta/templates/`](meta/templates/). Use **Bases**
for dashboards (unverified queue, missing-provenance, etc.). Details: [`meta/obsidian.md`](meta/obsidian.md).

## 3. (Optional) Connect an agent via MCP — while Obsidian is open
Follow [`meta/mcp-setup.md`](meta/mcp-setup.md): install *Local REST API* (≥5.5.2) → `export OBSIDIAN_API_KEY=…` →
`cp .mcp.json.example .mcp.json` → verify with `/mcp`. Read-only tools are pre-set in
[`.claude/settings.json`](.claude/settings.json). **MCP is optional** — agents work from files + Git without it.

## 4. (Optional) Use the specialist agent
For guided help (Obsidian, MCP, intake, retrieval), see the provider-neutral spec
[`meta/specialist.md`](meta/specialist.md). In Claude Code/Codex it's packaged as the `knowledge-ops` skill
(`.claude/skills/knowledge-ops/`). It advises and prepares changes; **it never marks knowledge `verified`.**

## 4b. (Optional) Let the base act on your projects
If your domain repo ships [`playbook/`](playbook/README.md), bootstrap each consumer project once:
`bash <knowledge-repo>/playbook/setup.sh project <project-dir> [--claude] [--codex]` and commit the generated files.
Teammates only clone both repos (side by side, or with a one-line `.env`). No per-machine install.

## 5. Feed it (structured intake)
Add material to [`inbox/`](inbox/) (optionally via [`meta/templates/capture.md`](meta/templates/capture.md)), then run
the intake loop in [`meta/workflows/intake.md`](meta/workflows/intake.md): triage → preserve originals to
[`sources/`](sources/) → synthesize own-words notes in [`notes/`](notes/) with citations and links → `status: proposed`.
Or ask an agent: *"ingest this URL"*, *"process my inbox"*. Not sure what a finished result looks like? Open the
[`examples/`](examples/) pair; [`SETUP.md`](SETUP.md) lists the "done when" criteria.

## 6. Review & publish (quality gate)
A **human** reviews the Git diff and promotes `proposed → verified` (or ADR `accepted`) using the
[validation checklist](meta/workflows/validation-checklist.md). Canonical knowledge enters `main` only via review
([`meta/workflows/git-governance.md`](meta/workflows/git-governance.md)).

## 7. Retrieve (token-efficient)
Search metadata/filenames → read the 1–3 most relevant notes → follow links → open sources only if needed → answer with
references. See [`meta/workflows/retrieval.md`](meta/workflows/retrieval.md).

---
**Stays true at every step:** plain Markdown + Git is the source of truth; Obsidian, MCP, and the specialist are
optional; nothing is `verified` without a human; no secrets in Git (`.mcp.json`/`.env` are gitignored).
