---
name: agent-memory-system
description: "Persistent long-term memory implementation for Claude Code using file-based memory stores, session extraction, and knowledge graphs. Use this skill any time Claude needs to remember things across sessions, persistent memory needs to be set up, or context loss prevention needs to be implemented. Trigger immediately on: \"memory\", \"remember this\", \"persistent memory\", \"agent memory\", \"memory system\", \"context loss\", \"save this for later\", \"memory graph\", \"knowledge graph\", \"session memory\", \"long-term memory\", \"memory extraction\", \"cross-session\". If someone says \"remember this for next time\" or \"set up memory for the agent\" this skill MUST trigger."
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
1. Hook sends a prompt to Claude Code
2. Claude asks: "Do you want to save a memory about this conversation?"
3. If yes, triggers REM Sleep background subagent
4. Subagent reads entire transcript, distills key insights
5. Stores insights in correct areas of persistent memory database

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
1. Create an Obsidian skill that teaches Claude Code frontmatter usage, Markdown file creation in your Vault, and backlink navigation
2. Set up slash commands for working with your Vault
3. Set up subagents for reading, creating, and organizing notes

## Git-Based Memory

Granular Git commits create a time-dimensioned memory:

- Commit as frequently as possible — each small functional change gets its own commit
- This gives the agent a realistic view of how work progressed
- Agent can traverse git history to see what changes were made, when, and where specific decisions were introduced

### Time Dimension
Combine Git with the memory database: memory files live in a Git repo, each addition committed with a timestamp. This creates a temporal narrative of learning and decisions.

## Data Quality Principles

The quality of your memory system determines the quality of your AI output:

1. **Collect data consistently** — Store observations about daily tasks
2. **Structure for both humans AND agents** — Files should be readable by both
3. **Domain-specific knowledge is your edge** — Everyone uses the same models; your unique data makes the difference
4. **Shape matters** — How you format and organize data directly affects agent performance
