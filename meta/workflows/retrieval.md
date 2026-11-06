# Workflow: Filesystem-first retrieval

How to find and read knowledge using only ordinary tools — **no Obsidian, no MCP, no index, no custom engine**. This
is the primary retrieval path; it works with Obsidian closed.

> Cost principle: the expensive part is the text you pull into context, not the search. **Return little; read
> progressively.** Prefer snippets and the 1–3 notes you actually need.

## 1. Search
Use `ripgrep` (`rg`) over `notes/` (add `sources/` only when you need the evidence itself). Combine content terms
with frontmatter filters:
```sh
rg -l -i "event decoupling" notes/            # files mentioning the topic
rg -l "^status: verified"   notes/            # only verified knowledge
rg -l "^type: concept"      notes/            # by content type
rg -l "^domain: <domain>"   notes/            # by domain (multi-domain repos)
rg -n -i "<term>" notes/ -C2                   # with surrounding context (snippets)
```
Intersect filters by piping file lists, e.g. verified concepts about a topic:
```sh
comm -12 <(rg -l "^status: verified" notes/ | sort) <(rg -l -i "<topic>" notes/ | sort)
```

## 2. Rank by trust
Prefer `verified` > `reviewed` > `proposed`. Treat `unreviewed` and `deprecated` with care (a `deprecated` note
should point you to its successor via `superseded-by`).

## 3. Read progressively
1. Read a candidate's **frontmatter + `summary`** first.
2. Open the **full body** of only the 1–3 notes that actually answer the question.
3. Stop when you have enough. Don't load the whole folder.

## 4. Traverse relationships
- Follow `related:` entries and `[[wikilinks]]` in the note.
- Find **backlinks** (what points *to* a note) by its id:
  ```sh
  rg -l "\[\[<note-id>\]\]" notes/
  ```
- The typed `related:` field is plain text, so it is fully greppable even where Obsidian/MCP backlink APIs aren't
  available.

## 5. Cite what you use
When you answer from a note, cite the note and the `sources:` entry its claim rests on.

## (Optional) via Obsidian MCP, when Obsidian is open
If a human is running Obsidian with the Local REST API plugin, you may use its `search_simple` (fuzzy) and
`search_query` (JsonLogic over frontmatter/tags) tools and `vault_read`. These return **capped** result sets (good
for context budget) but offer **lexical + metadata search only — no semantic search**, and are **unavailable when
Obsidian is closed**. See [`../mcp-setup.md`](../mcp-setup.md). The filesystem path above remains the default.

## Signs the filesystem is NOT enough (when to propose more)
Only a **measured** failure justifies revisiting the "not building" list (custom index/embeddings/etc.). Watch for:
- relevant notes **consistently missed** by lexical search;
- poor recall across **different terminology** for the same idea (vocabulary mismatch);
- **indirect relationships** you cannot surface via links/backlinks;
- **excessive context** consumed per query despite progressive reading;
- a hard requirement to retrieve **without Obsidian** that the filesystem genuinely can't meet.

If one of these is real and repeated, write it up (with examples) and propose the **smallest** addition — do not add
infrastructure for convenience.
