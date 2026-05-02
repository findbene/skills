---
name: agentic-workspace-setup
description: "Agent-friendly project workspace setup covering .agentic/ directory creation, progress.md, plan.md, discovery.md, and context recovery infrastructure. Use this skill any time a new project needs to be set up for agentic development, the .agentic/ workspace is missing, or context recovery files need to be created. Trigger immediately on: \".agentic\", \"workspace setup\", \"agentic workspace\", \"progress.md\", \"plan.md\", \"discovery.md\", \"context recovery\", \"project setup\", \"agentic project\", \"new project setup\", \"agent workspace\", \"session tracking\". If someone says \"set up this project for agent development\" this skill MUST trigger."
---

# Agentic Workspace Setup

Every non-trivial project needs four essential documents for effective agent-driven development. These files form the backbone of context engineering — they allow brand new agent sessions to pick up exactly where previous sessions left off.

## The Four Essential Files

### 1. `discovery.md` — Intent & Requirements Discovery

**When:** Before any planning or coding begins.

**Process:**
- Conduct a structured Q&A session with the user
- Ask at least a few dozen detailed, pointed questions to deeply understand intent, goals, constraints, target users, and success criteria
- Store every question and answer verbatim

Discovery is the source of truth for what the project is supposed to achieve. All downstream planning references this document. Skipping discovery leads to building the wrong thing — the agent often has better domain knowledge than the user, so thorough questioning surfaces important considerations the user might miss.

### 2. `research.md` — Parallel Domain Research

**When:** Immediately after discovery is complete.

**Process:**
- Deploy multiple background subagents in parallel, each researching a specific domain:
  - Database patterns
  - API design
  - UI/UX patterns
  - Relevant libraries/frameworks
  - Existing implementations on GitHub
  - Documentation on the web
- Store each agent's verbatim findings, organized by domain section

Parallel research keeps the main context window clean while gathering comprehensive technical intelligence. Decisions grounded in real-world patterns are stronger than assumptions.

### 3. `plan.md` — Comprehensive Implementation Plan

**When:** After discovery and research are complete.

**Process:**
- Synthesize `discovery.md` and `research.md` into a detailed, phased plan
- Include numbered phases with clear objectives
- Specify tasks within each phase with dependencies
- Define acceptance criteria for each task
- Document technical approach and architecture decisions
- Include validation tools and strategies the agent will use
- Go back and forth with the user to refine — do not start implementing until user explicitly approves

A well-scoped plan prevents scope creep, wasted effort, and architectural dead ends.

### 4. `progress.md` — Implementation Tracking & Context Recovery

**When:** Created when implementation begins. Updated continuously.

**Process:**
- Summarize all phases and tasks from `plan.md`
- Track status of each task (pending / in-progress / completed / blocked)
- Record key decisions made during implementation
- Document current state and next steps
- Note any deviations from the original plan and why
- Update after completing a task or making a significant decision

This is the lifeline for context recovery. When the context window compacts or a fresh session starts, the new agent reads `progress.md` to understand exactly where the previous session left off. Without this, work gets repeated or lost.

## Agent-First Project Setup Principles

1. **90% of effort in planning, 10% in execution** — Spend most time in research, discovery, and planning phases
2. **Context documents over context window** — Store important information in external files, not in chat history
3. **Breadcrumb references** — Files should reference each other so agents can navigate between related context
4. **Agent-centric validation** — Discuss in the planning phase what tools the agent has to validate its own work

## Quick Start

When starting a new project:
1. Create a `.agentic/` directory (or project-level equivalent)
2. Begin discovery phase — ask the user comprehensive questions
3. Deploy parallel research subagents
4. Synthesize into plan with user approval
5. Begin implementation with progress tracking
