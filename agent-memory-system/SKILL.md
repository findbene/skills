---
name: agent-memory-system
description: 'Persistent long-term memory implementation for Claude Code using file-based memory stores, session extraction,. Triggers: "use agent-memory-system", "build agent memory system", "agent memory system".'
allowed-tools: Glob, Grep, Read
---

# Agent Memory System

LLMs are stateless — they forget everything between sessions. This skill implements persistent memory through external file-based systems, turning ephemeral conversations into durable knowledge.

## Three Core Memory Skills

### Skill 1: Memory (Storage)
A hierarchical directory tree for organized memory storage. Depth corresponds to specificity — go deeper into folders for more niche memories.

```
memory/
├── projects/
│   ├── project-a/
│   │   ├── architecture-decisions.md
│   │   └── key-learnings.md
│   └── project-b/
├── personal/
│   ├── preferences.md
│   └── workflows.md
├── technical/
│   ├── patterns.md
│   └── tools.md
└── business/
    ├── contacts.md
    └── processes.md
```

### Skill 2: REM Sleep (Memory Extraction)
Deploy a background subagent to read the conversation transcript, distill key insights, and store them in the correct areas of the memory tree.

Trigger this:
- Before context window compacts (~80% context usage)
- At end of work sessions
- When switching between major topics

The analogy to biological sleep is intentional — just as the brain consolidates short-term memories during REM sleep, this process converts session context into durable stored knowledge.

### Skill 3: Recall (Memory Retrieval)
Deploy a background subagent to search the memory tree, find memories relevant to the current project/task, and pass them back to the main agent. Use at the start of new sessions or when context is needed.

## Hook-Based Automatic Memory

Set up a hook that monitors context window usage:

**Trigger:** ~80% of context consumed (before auto-compact)

**Action:**
1. Hook sends a prompt to Claude Code → verify: step output matches expected outcome
2. Claude asks: "Do you want to save a memory about this conversation?" → verify: user confirms
3. If yes, triggers REM Sleep background subagent → verify: step output matches expected outcome
4. Subagent reads entire transcript, distills key insights → verify: file content matches expected shape
5. Stores insights in correct areas of persistent memory database → verify: step output matches expected outcome

This prevents memory loss during context compaction — the single biggest cause of lost work in long agent sessions.

## Obsidian Integration

Obsidian works well for Claude Code memory because it uses local Markdown files (no cloud dependency, no MCP server needed).

### Frontmatter Backlinks
Frontmatter metadata creates a network of related notes:

```yaml
---
tags: [project-a, architecture]
backlinks:
  - "[[database-design]]"
  - "[[api-patterns]]"
  - "[[deployment-strategy]]"
---
```

Backlinks enable semantic navigation — when Claude reads a note, it knows where to find related information without searching the entire tree.

### Setup Steps
1. Create an Obsidian skill that teaches Claude Code frontmatter usage, Markdown file creation in your Vault, and backlink navigation → verify: output exists + parses without error
2. Set up slash commands for working with your Vault → verify: step output matches expected outcome
3. Set up subagents for reading, creating, and organizing notes → verify: file content matches expected shape

## Git-Based Memory

Granular Git commits create a time-dimensioned memory:

- Commit as frequently as possible — each small functional change gets its own commit
- This gives the agent a realistic view of how work progressed
- Agent can traverse git history to see what changes were made, when, and where specific decisions were introduced

### Time Dimension
Combine Git with the memory database: memory files live in a Git repo, each addition committed with a timestamp. This creates a temporal narrative of learning and decisions.

## Data Quality Principles

The quality of your memory system determines the quality of your AI output:

1. **Collect data consistently** — Store observations about daily tasks → verify: user confirms
2. **Structure for both humans AND agents** — Files should be readable by both → verify: file content matches expected shape
3. **Domain-specific knowledge is your edge** — Everyone uses the same models; your unique data makes the difference → verify: step output matches expected outcome
4. **Shape matters** — How you format and organize data directly affects agent performance → verify: step output matches expected outcome

## When NOT to use

- Single-session work that fits in current context — no persistence needed
- Cross-project user preferences/facts → use `~/.claude/memory/` directly (the `remember` skill)
- Project-state and active task tracking → use `.agentic/progress.md`
- Throwaway scratch notes during a debugging session — TaskCreate is enough
- Secrets, credentials, or PII — memory files are not a secret store

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll just dump the whole transcript as memory" | Raw transcripts rot context; distill into structured notes |
| "One giant memory.md file is fine" | Flat files don't scale; hierarchy + backlinks make recall cheap |
| "Skip the REM-sleep extraction this once" | The session you skip is the one with the key decision you'll need later |
| "Memory is the same as progress.md" | Memory = durable cross-session knowledge. Progress = current task state. Don't merge them. |

## Output Contract

Done when:
- Hierarchical memory tree exists with at least projects/, personal/, technical/, business/ branches
- REM-sleep extraction subagent or hook is wired and triggers near context limit
- Recall subagent path is defined for session startup
- Frontmatter convention (tags + backlinks) documented for the vault
- Git commit cadence chosen so memory has a time dimension
- No secrets stored in the tree

## Examples

### Example 1 — End-of-session extraction
- Input: User finished a long debugging session on project-a's auth flow; context is near 80%
- Action: Trigger REM-sleep subagent to read the transcript, extract decisions ("we chose refresh-token rotation over sliding sessions because..."), write to `memory/projects/project-a/architecture-decisions.md` with backlinks to related notes
- Output: New/updated memory file committed with timestamp; next session's Recall subagent will surface this when project-a is opened

### Example 2 — Fresh session on a known project
- Input: User opens a new Claude Code session on project-b
- Action: Recall subagent searches `memory/projects/project-b/` and its backlinked notes, returns a digest of decisions, gotchas, and current state to the main agent's first turn
- Output: Main agent starts with full prior context without burning the user's tokens on re-explaining
