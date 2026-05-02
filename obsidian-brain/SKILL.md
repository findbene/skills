---
name: obsidian-brain
description: "Obsidian vault connector bridging Claude Code sessions with Biniyam persistent second-brain memory at C:/Users/findb/OneDrive/Documents/Obsidian_Biniyam/. Use this skill any time Obsidian context needs to be loaded at session start, memories or decisions need to be saved to the vault, project progress needs to be synced, or the vault needs to be searched for relevant knowledge. Trigger immediately on: \"Obsidian\", \"check Obsidian\", \"save to Obsidian\", \"update my vault\", \"Obsidian brain\", \"load from vault\", \"write to my notes\", \"my second brain\", \"vault\", \"session memory\", \"load context\", \"save this to memory\", \"obsidian-brain\". If any session starts or someone says \"save this to memory\" this skill MUST trigger."
---

# Obsidian Brain

Biniyam's Obsidian vault is the persistent second brain for Claude Code. Every project, decision, and learned pattern lives here — bridging sessions, compaction events, and context resets.

Because Obsidian uses plain local Markdown files, no MCP server is required. Claude Code reads and writes vault files directly using Read, Write, Edit, and Grep.

## Vault Location

```
VAULT = C:\Users\findb\OneDrive\Documents\Obsidian_Biniyam\Biniyam-Obsidian
```

**Always use this path.** Never prompt Biniyam to confirm the vault location.

---

## Vault Folder Map

| Folder | Purpose | When Claude uses it |
|--------|---------|-------------------|
| `00-Inbox/` | Quick captures, unprocessed ideas | Drop raw notes mid-session |
| `01-Notes/Ideas/` | Concepts, hypotheses, new directions | Save emerging ideas |
| `01-Notes/Work/` | Work-related notes | Save meeting notes, decisions |
| `01-Notes/Personal/` | Personal notes | N/A — do not write here |
| `02-Documents/` | File attachments | N/A |
| `03-Projects/<name>/` | Project-specific notes, decisions, progress | PRIMARY memory store |
| `04-References/Articles/` | Research articles, links | Save external research |
| `04-References/Books/` | Book notes | N/A |
| `05-Templates/` | Note templates | Read for formatting guidance |
| `99-Archive/` | Completed/inactive projects | Move stale notes here |

### Project Folders (Pre-existing)
- `03-Projects/AI_Agency/` — Main AI agency project (matches `C:\AI_Agency`)
- `03-Projects/AmazonAM/` — remotetechgear.com affiliate site
- `03-Projects/SosinaMart/` — Next.js e-commerce project
- `03-Projects/Lidia/` — Wedding website

---

## Installed Plugins (Relevant to Claude)

- **Dataview** — SQL-like queries over notes. Write `dataview` frontmatter fields and Claude can query them.
- **Periodic Notes** — Daily/weekly notes with templated structure.
- **Obsidian Tasks** — Task tracking with `- [ ]` syntax and metadata.
- **Templater** — Dynamic templates (Claude creates static output; Templater runs in Obsidian UI).
- **Omnisearch** — Full-text vault search (Claude uses Grep instead — same result).
- **Obsidian Git** — Auto-syncs vault to Git.

---

## Standard Frontmatter

Every note Claude creates or updates must include this frontmatter:

```yaml
---
date: YYYY-MM-DD
project: <project-name>
tags: [<relevant-tags>]
type: <note-type>
source: claude-code
---
```

**Type values:** `decision`, `memory`, `progress`, `research`, `daily`, `inbox`, `reference`

**This matters** because Dataview uses these fields for queries, and Obsidian's graph and tag views depend on them.

---

## Operations

### 1. RECALL — Load Context at Session Start

Run this when starting any session on a project:

```
1. Read 03-Projects/<project>/claude-memory.md  (Claude's persistent memory for that project)
2. Read 03-Projects/<project>/_decisions.md      (key architectural decisions)
3. Grep VAULT/03-Projects/<project>/ for recent notes (last 7 days by date frontmatter)
4. Read 00-Inbox/ — any unprocessed notes for the project
```

Combine what you find with `.agentic/progress.md` for a complete session orientation.

**Example:**
```python
# Pseudo-code for what Claude does
vault = "C:/Users/findb/OneDrive/Documents/Obsidian_Biniyam/Biniyam-Obsidian"
project = "AI_Agency"
read(f"{vault}/03-Projects/{project}/claude-memory.md")
read(f"{vault}/03-Projects/{project}/_decisions.md")
grep(f"{vault}/03-Projects/{project}/", pattern="2026-03")  # current month
```

### 2. SAVE MEMORY — Write Persistent Memory

When completing a task, making a decision, or ending a session:

**Target file:** `03-Projects/<project>/claude-memory.md`

This is Claude's running memory file for the project. Append to it — never overwrite. Keep it under 500 lines; archive old entries to `99-Archive/` when it gets long.

```markdown
## 2026-03-15 — Session Memory

**Completed:** <what was built>
**Decisions:** <key choices made and why>
**Blockers:** <anything unresolved>
**Next:** <immediate next steps>
**Files changed:** <key files modified>
```

### 3. SAVE DECISION — Record Architecture Decisions

When making any significant technical or strategic choice:

**Target file:** `03-Projects/<project>/_decisions.md`

```markdown
## DECISION-<N>: <title>
- **Date:** YYYY-MM-DD
- **Context:** <what situation prompted this>
- **Decision:** <what was decided>
- **Alternatives considered:** <what else was evaluated>
- **Rationale:** <why this choice>
- **Consequences:** <what this implies for future work>
```

### 4. SYNC PROGRESS — Update Daily Note

At the end of any meaningful work session, write a summary to that day's periodic note.

**Target file:** `03-Projects/<project>/Daily/<YYYY-MM-DD>.md`

Create the `Daily/` subfolder inside the project folder if it doesn't exist.

```markdown
---
date: YYYY-MM-DD
project: AI_Agency
tags: [daily, progress]
type: daily
source: claude-code
---

# <Project> — <YYYY-MM-DD>

## What happened
<2-3 sentence summary of the session>

## Completed
- <task 1>
- <task 2>

## Decisions made
- <decision summary>

## Next session
- <top 3 next actions>

## Links
- [[claude-memory]] | [[_decisions]]
```

### 5. SEARCH VAULT — Find Relevant Knowledge

Use Grep to search across the vault before assuming Claude doesn't know something:

```
Grep VAULT pattern="<keyword>" glob="**/*.md"
```

Search strategies:
- Search by tag: `grep "tags:.*<tag>"` in frontmatter
- Search by project: `grep "project: <name>"`
- Search by topic: `grep "<keyword>"` across all `.md` files
- Search decisions: target `_decisions.md` files specifically

### 6. INBOX DROP — Quick Capture Mid-Session

When an idea, link, or observation comes up that's not the current focus:

**Target file:** `00-Inbox/<YYYY-MM-DD>-<slug>.md`

```markdown
---
date: YYYY-MM-DD
project: <project or "general">
tags: [inbox]
type: inbox
source: claude-code
---

<Quick note content>
```

Review inbox notes at the start of the next session and file them properly.

---

## Session Workflow

### Session Start (RECALL)
```
1. Read 03-Projects/<project>/claude-memory.md
2. Read 03-Projects/<project>/_decisions.md
3. Scan 00-Inbox/ for anything flagged for this project
4. Merge with .agentic/progress.md
5. Summarize state to user in 3-5 bullets
```

### During Session (CAPTURE)
- Drop ideas to `00-Inbox/` as they arise
- Save decisions immediately to `_decisions.md`
- Don't wait until end of session — capture while context is fresh

### Session End (SYNC)
```
1. Append session summary to claude-memory.md
2. Write daily note to 03-Projects/<project>/Daily/<date>.md
3. Update .agentic/progress.md (this is still the authoritative recovery doc)
4. Clear anything from 00-Inbox that belongs in the project folder
```

---

## Claude-Brain Folder (Cross-Project Intelligence)

For knowledge that applies across ALL projects, use a dedicated folder:

**Path:** `01-Notes/Work/Claude-Brain/`

Create this folder if it doesn't exist. Files:
- `patterns.md` — recurring code patterns, architecture insights
- `tools.md` — tool behaviors, gotchas, API quirks
- `preferences.md` — Biniyam's workflow preferences, communication style
- `lessons.md` — mistakes made, corrections, things that didn't work

This mirrors `~/.claude/projects/C--AI-Agency/memory/` but lives inside Obsidian for unified access.

---

## File Naming Convention

```
claude-memory.md          → running memory (append-only)
_decisions.md             → architecture decision log (prefix _ = always-open)
Daily/YYYY-MM-DD.md       → daily session notes
YYYY-MM-DD-<topic>.md     → dated reference notes
<topic>.md                → evergreen reference notes (no date prefix)
```

---

## What NOT to Write to Obsidian

- Secrets, API keys, .env content — never
- Raw code files — keep code in the repo, write summaries to Obsidian
- Temporary debug output — that belongs in terminal, not vault
- Duplicate of what's already in .agentic/progress.md — summarize and link instead

---

## Quick Reference

```
VAULT root:    C:\Users\findb\OneDrive\Documents\Obsidian_Biniyam\Biniyam-Obsidian
Project notes: VAULT\03-Projects\<project>\
Memory file:   VAULT\03-Projects\<project>\claude-memory.md
Decisions:     VAULT\03-Projects\<project>\_decisions.md
Daily notes:   VAULT\03-Projects\<project>\Daily\YYYY-MM-DD.md
Cross-project: VAULT\01-Notes\Work\Claude-Brain\
Inbox:         VAULT\00-Inbox\
```
