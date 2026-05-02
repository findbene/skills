---
name: agent-priming
description: "Claude Code session priming specialist for loading project context, git history, and architecture understanding before starting work. Use this skill any time a new session is starting and context needs to be loaded, project state needs to be understood, or session orientation is needed before writing code. Trigger immediately on: \"prime the session\", \"load context\", \"what is this project\", \"get up to speed\", \"orient yourself\", \"start of session\", \"new session\", \"read the project\", \"understand the codebase\", \"session start\", \"project context\", \"catch me up\", \"what was I working on\". If a session starts without context this skill MUST trigger."
---

# Agent Priming

Priming means loading the right context into the agent's context window before work begins, so the agent knows the project's current state, architecture decisions, and available tools before writing any code.

## Why Prime?

Without priming, the agent:
- Doesn't know the project's current state
- May duplicate work already completed
- Misses important architecture decisions
- Doesn't know which tools/validation methods are available
- Produces generic rather than project-specific outputs

## The `/prime` Slash Command

Create a local prime command for each project (or globally for general patterns):

### What `/prime` Should Do
1. **Look around the project** — Read key configuration and context files
2. **Load relevant skills** — Activate project-specific skills
3. **Load relevant files** — Read architecture docs, recent changes
4. **Check Git history** — Review last few commits to understand current state
5. **Web research** (optional) — Gather updated info on libraries or APIs being used
6. **Read progress file** — Know exactly where the last session left off

### Example `/prime` Implementation
```
When invoked, do the following in sequence:

1. Read CLAUDE.md for project configuration
2. Read .agentic/progress.md for current state
3. Read .agentic/plan.md for the overall roadmap
4. Run `git log --oneline -10` to see recent commits
5. Run `git diff HEAD~3` to see recent changes
6. Load any project-specific skills referenced in CLAUDE.md
7. Summarize current state and ask what to work on next
```

## Git History as Context

Include references to past Git commits in your prime workflow — the agent sees the last few commits, understands the project stage, identifies recently modified files, and knows what was just completed vs what's pending.

## Priming Patterns

### Session Start Priming
At the beginning of every new session:
1. Read progress tracking documents
2. Load relevant skills
3. Check recent git history
4. Identify current phase and next tasks

### Feature Implementation Priming
Before starting a new feature:
1. Deploy background subagent to analyze existing codebase for relevant patterns
2. Deploy research agents for documentation and GitHub examples
3. Load the feature's task file from the plan
4. Review related completed tasks for context

### Context Recovery Priming
When resuming after context compaction:
1. Read `progress.md` immediately
2. Check what tasks are marked complete
3. Read the next pending task's full description
4. Review git log since last session
5. Resume work with full awareness

### Research Priming
Before tackling an unfamiliar domain:
1. Deploy parallel research subagents
2. Use NotebookLM MCP for deep research compilation
3. Store findings in research documents
4. Load relevant findings into context

## Global vs Local Priming

### Global Prime (in `~/.claude/`)
- General patterns that apply to all projects
- Loading user preferences and work style
- Checking global memory for relevant context
- Universal tool setup verification

### Local Prime (in project directory)
- Project-specific context loading
- Reading project's unique planning documents
- Loading project-specific skills
- Checking project's git history and state
