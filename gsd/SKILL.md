---
name: gsd
description: "Get Shit Done meta-prompting and context engineering for creating specs quickly, breaking down work, and executing with clarity. Use this skill any time work needs to be organized and executed efficiently, specs need to be written fast, or a focused execution mode is needed without over-planning. Trigger immediately on: \"GSD\", \"get shit done\", \"just do it\", \"let us just build\", \"just ship it\", \"make it happen\", \"move fast\", \"write the spec\", \"just build it\", \"focus mode\", \"execution mode\", \"no fluff\", \"just get it done\", \"stop planning\". If someone says \"stop planning, let us just build\" or \"GSD mode\" this skill MUST trigger."
---

# GSD (Get Shit Done) - Context Engineering System

A system that solves **context rot** — the quality degradation that happens as Claude fills its context window.

## How It Solves Context Rot

**Problem:** Quality degrades as context accumulates.

**GSD's Solution:**
1. **Fresh 200k contexts per task** - Executors run clean, main session stays responsive
2. **Persistent project memory** - State lives in markdown files, not chat history
3. **Parallel orchestration** - Work happens in subagents; your session never accumulates garbage
4. **Atomic tasks** - Each task self-contained with explicit verification

## Installation

```bash
npx get-shit-done-cc --claude --global
```

## Core Workflow

### 1. Initialize (`/gsd:new-project`)
- Questions until understanding complete
- Parallel research agents investigate domain
- Creates: `PROJECT.md`, `REQUIREMENTS.md`, `ROADMAP.md`, `STATE.md`

### 2. Discuss (`/gsd:discuss-phase [N]`)
- Capture implementation preferences before planning
- Ask deep questions about gray areas
- Creates: `{phase}-CONTEXT.md`

### 3. Plan (`/gsd:plan-phase [N]`)
- Research implementation approaches
- Create 2-3 atomic task plans in XML
- Creates: `{phase}-RESEARCH.md`, `{phase}-{N}-PLAN.md`

### 4. Execute (`/gsd:execute-phase [N]`)
- Run plans in parallel waves with fresh contexts
- Atomic git commits per task

### 5. Verify (`/gsd:verify-work [N]`)
- Manual UAT to confirm deliverables
- Auto-diagnose failures, create fix plans

## Project Memory Structure

| File | Purpose |
|------|---------|
| `PROJECT.md` | Vision (always loaded) |
| `REQUIREMENTS.md` | Scoped requirements |
| `ROADMAP.md` | Progress tracking |
| `STATE.md` | Decisions, blockers, memory |
| `PLAN.md` | Atomic task with XML structure |

## Task XML Structure

```xml
<task type="auto">
  <name>Create login endpoint</name>
  <files>src/app/api/auth/login/route.ts</files>
  <action>Use jose for JWT, validate credentials</action>
  <verify>curl POST localhost:3000/api/auth/login returns 200</verify>
  <done>Valid credentials return cookie, invalid return 401</done>
</task>
```

## Commands

| Command | Purpose |
|---------|---------|
| `/gsd:new-project` | Initialize project |
| `/gsd:discuss-phase [N]` | Capture preferences |
| `/gsd:plan-phase [N]` | Create execution plans |
| `/gsd:execute-phase [N]` | Run plans in fresh contexts |
| `/gsd:verify-work [N]` | Confirm deliverables |
| `/gsd:quick` | Ad-hoc work without full cycle |
| `/gsd:debug` | Systematic issue investigation |
| `/gsd:progress` | Check current state |

## Multi-Agent Orchestration

- **Research:** 4 parallel agents investigate stack, features, architecture
- **Planning:** Planner + checker iterate until verification passes
- **Execution:** Parallel executors in fresh contexts; main at ~30-40%
- **Verification:** Auto-diagnose failures; debug agents find root causes
