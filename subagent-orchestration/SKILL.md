---
name: subagent-orchestration
description: "Subagent deployment and orchestration patterns for Claude Code including parallel execution, background agents, team coordination, and multi-agent workflows. Use this skill any time multiple agents need to be deployed in parallel, complex tasks need to be distributed to subagents, background agent patterns need to be used, or multi-agent workflows need to be orchestrated. Trigger immediately on: \"subagent\", \"parallel agents\", \"background agent\", \"launch agents\", \"multi-agent\", \"agent orchestration\", \"spawn agent\", \"deploy agents\", \"parallel tasks\", \"agent team\", \"run in background\", \"coordinate agents\", \"agent workflow\", \"task distribution\". If someone says \"run multiple agents in parallel\" or \"orchestrate this with subagents\" this skill MUST trigger."
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

1. **Fresh context per task** — Each subagent gets a clean 200k context window
2. **Externalized state** — Progress lives in markdown files, not chat history
3. **Parallel when possible** — Deploy independent work simultaneously
4. **Sequential when dependent** — Use progress tracking for ordered execution
5. **Atomic commits** — Each subtask committed separately
