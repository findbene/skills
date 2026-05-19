---
name: subagent-orchestration
description: 'Subagent deployment and orchestration patterns for Claude Code including parallel execution, background agents, team. Triggers: "use subagent-orchestration", "subagent orchestration", "subagent task".'
allowed-tools: Edit, Glob, Grep, Read
---

# Subagent Orchestration

## Core Mental Model: Skills vs Subagents

**Critical distinction:**
- **Subagents** = Fresh context windows. Their purpose is to **protect the main context window from pollution**, not to assign roles.
- **Skills** = Information packages that specialize an agent for a specific task. More customizable than hardcoded personas because they can be chained.

**Correct pattern:** Deploy general subagents directed at specific skill folders to specialize them.

## Top 4 Background Subagent Use Cases

### 1. Context Priming & Parallel Research
Deploy several parallel agents to research: existing docs, GitHub implementations, library best practices, related patterns in codebase.

### 2. Memory Extraction
When approaching context limits: deploy background subagent to extract key decisions, blockers, current state into working memory files.

### 3. Server Log Monitoring
Deploy dev server as background task, deploy monitoring agent to watch logs and flag errors in real-time.

### 4. Background Test Writing
Deploy background agent to read the plan/spec and write tests in parallel with main implementation.

## Task-Based Orchestration Pattern

### Step 1: Create Phased Plan Document
One source-of-truth document with all phases and sub-tasks, referencing discovery and research.

### Step 2: Shard Into Task Files
Each task file specifies: how it fits the larger plan, implementation steps, files to create/edit, validation steps, atomic git commit at completion.

### Step 3: Configure Agent Environment
Create `progress.md` summarizing all subtasks. Subagents mark tasks as completed. Edit `CLAUDE.md` to reference progress, plan, and task directory.

### Step 4: Execute
Subagents complete tasks in clean context windows. Main agent stays light (just orchestration). Each completed subtask = atomic git commit.

## Claude Code Tasks Feature

Tasks feature turns todo items into task files:
- Multiple agents work on tasks simultaneously
- Subagents can add new tasks if they discover issues
- Configure `TASKS` env var in `settings.json` to point multiple agents to same task directory

## Key Principles

1. **Fresh context per task** — Each subagent gets a clean 200k context window → verify: user confirms
2. **Externalized state** — Progress lives in markdown files, not chat history → verify: step output matches expected outcome
3. **Parallel when possible** — Deploy independent work simultaneously → verify: step output matches expected outcome
4. **Sequential when dependent** — Use progress tracking for ordered execution → verify: step output matches expected outcome
5. **Atomic commits** — Each subtask committed separately → verify: git status clean

## When NOT to use

- Single-step task that fits in current context — no point in subagent overhead
- Task requires conversation state the subagent does not have — keep on main
- Production-critical reasoning that needs Opus review — delegate cheap, review on Opus
- Trivial one-line edit — direct Edit tool is faster
- User explicitly asks you to do it yourself

## Red Flags

| Rationalization | Reality |
|---|---|
| "Subagent will figure out the context" | Subagent has a fresh context — pass all needed info or it will hallucinate |
| "Skip the review of subagent output" | Mandatory per model-routing.md — Opus reviews every worker output |
| "Run them sequentially to play it safe" | Independent tasks should be parallel — sequential wastes time |
| "Pick Opus for everything to be safe" | Wastes cost; route by tier (Haiku for bulk, Sonnet for code, Opus for arch/review) |

## Output Contract

Finished output must contain:
- Task classification (T0/T1/T2/T3) per `model-routing.md`
- Subagent prompt is self-contained (all paths, context, success criteria)
- Parallel vs sequential decision documented
- Output reviewed by Opus before merging back
- Atomic commits per subtask, not one bulk commit
- Failure handling specified (retry, fallback, escalate to user)

## Examples

**Example 1 — Parallel research before greenfield feature**
- Input: "We are building a new payments feature"
- Action: Spawn 4 parallel Haiku subagents (existing codebase patterns, Stripe docs, competitor analysis, regulatory) → collect outputs → Opus synthesizes
- Output: Research bundle in `.agentic/research.md`, 4 subagent transcripts, Opus-reviewed synthesis

**Example 2 — Background test writing**
- Input: "Implement new endpoint" (main agent) + parallel test writer
- Action: Spawn Sonnet subagent to read spec and write tests in parallel → main agent implements → Opus reviews both outputs before commit
- Output: Endpoint + tests committed in same PR, both reviewed
