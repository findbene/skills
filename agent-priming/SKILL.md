---
name: agent-priming
description: 'Claude Code session priming specialist for loading project context, git history, and architecture understanding before starting w. Triggers: "use agent-priming", "build agent priming", "agent priming.'
allowed-tools: Glob, Grep, Read
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
1. **Look around the project** — Read key configuration and context files → verify: file content matches expected shape
2. **Load relevant skills** — Activate project-specific skills → verify: file content matches expected shape
3. **Load relevant files** — Read architecture docs, recent changes → verify: file content matches expected shape
4. **Check Git history** — Review last few commits to understand current state → verify: all checks pass
5. **Web research** (optional) — Gather updated info on libraries or APIs being used → verify: step output matches expected outcome
6. **Read progress file** — Know exactly where the last session left off → verify: file content matches expected shape

### Example `/prime` Implementation
```
When invoked, do the following in sequence:

1. Read CLAUDE.md for project configuration → verify: file content matches expected shape
2. Read .agentic/progress.md for current state → verify: file content matches expected shape
3. Read .agentic/plan.md for the overall roadmap → verify: file content matches expected shape
4. Run `git log --oneline -10` to see recent commits → verify: command exit code 0
5. Run `git diff HEAD~3` to see recent changes → verify: command exit code 0
6. Load any project-specific skills referenced in CLAUDE.md → verify: file content matches expected shape
7. Summarize current state and ask what to work on next → verify: user confirms
```

## Git History as Context

Include references to past Git commits in your prime workflow — the agent sees the last few commits, understands the project stage, identifies recently modified files, and knows what was just completed vs what's pending.

## Priming Patterns

### Session Start Priming
At the beginning of every new session:
1. Read progress tracking documents → verify: file content matches expected shape
2. Load relevant skills → verify: file content matches expected shape
3. Check recent git history → verify: all checks pass
4. Identify current phase and next tasks → verify: user confirms

### Feature Implementation Priming
Before starting a new feature:
1. Deploy background subagent to analyze existing codebase for relevant patterns → verify: step output matches expected outcome
2. Deploy research agents for documentation and GitHub examples → verify: step output matches expected outcome
3. Load the feature's task file from the plan → verify: file content matches expected shape
4. Review related completed tasks for context → verify: user confirms

### Context Recovery Priming
When resuming after context compaction:
1. Read `progress.md` immediately → verify: file content matches expected shape
2. Check what tasks are marked complete → verify: all checks pass
3. Read the next pending task's full description → verify: file content matches expected shape
4. Review git log since last session → verify: step output matches expected outcome
5. Resume work with full awareness → verify: step output matches expected outcome

### Research Priming
Before tackling an unfamiliar domain:
1. Deploy parallel research subagents → verify: step output matches expected outcome
2. Use NotebookLM MCP for deep research compilation → verify: step output matches expected outcome
3. Store findings in research documents → verify: step output matches expected outcome
4. Load relevant findings into context → verify: file content matches expected shape

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

## When NOT to use

- Task is unrelated to agent priming — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Agent Priming needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for agent priming
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving agent priming
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
