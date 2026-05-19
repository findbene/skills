---
name: agent-planning-execution
description: 'Structured planning and execution methodology for AI coding agents covering task decomposition,. Triggers: "use agent-planning-execution", "build agent planning execution", "agent planning execution".'
allowed-tools: Glob, Grep, Read
---

# Agent Planning & Execution

A context engineering system that prevents **context rot** — the quality degradation that happens as Claude fills its context window.

## Core Concept

Segment your project into tasks, each accomplished by a subagent with its own fresh context window. This keeps every execution context clean and prevents the cumulative quality loss that comes from overloaded context windows.

### Key Mechanisms
1. **Fresh 200k contexts per task** — Each executor runs in a clean context → verify: command exit code 0
2. **Persistent project memory** — State lives in markdown files, not chat history → verify: step output matches expected outcome
3. **Parallel orchestration** — Work happens in subagents; main session stays responsive → verify: step output matches expected outcome
4. **Atomic tasks** — Each task is self-contained with explicit verification → verify: user confirms
5. **Atomic Git commits** — Each completed task committed separately for history/rollback → verify: git status clean

## Phased Execution Workflow

### Phase 1: Research & Discovery
1. Ask extremely detailed, pointed questions about the project → verify: step output matches expected outcome
2. Conduct comprehensive deep research (web search, GitHub implementations, documentation) → verify: step output matches expected outcome
3. Store all findings in external research documents → verify: step output matches expected outcome

The agent often has better domain knowledge than the user — thorough questioning surfaces considerations the user might miss.

### Phase 2: Planning
1. Review all Q&A and research documents → verify: step output matches expected outcome
2. Synthesize into a phased plan → verify: step output matches expected outcome
3. Consider the agent's environment: available tools, validation methods, testing capabilities → verify: all tests pass
4. Create one source of truth document with all phases and subtasks → verify: output file exists + no syntax error
5. Reference exact Q&A and research documents for each task → verify: step output matches expected outcome

### Phase 3: Task Sharding
Break the plan into individual subtask documents organized in a task directory with phase subdirectories. Each task file includes:
- How this subtask fits the larger plan (short summary)
- Specific implementation steps
- Files to create/edit
- Connection to previously completed and future subtasks
- Validation steps using available tools
- Atomic git commit instruction

### Phase 4: Orchestrated Execution
1. Configure progress tracking file (summary of all subtasks) → verify: step output matches expected outcome
2. Main agent deploys subagents for each subtask sequentially → verify: step output matches expected outcome
3. Subagents mark completed tasks in progress file → verify: step output matches expected outcome
4. If main context compacts, read progress file to resume → verify: file readable + content matches expected shape
5. Main context stays light (only orchestration overhead) → verify: step output matches expected outcome

## Agent-Centric Validation

During planning, explicitly discuss what validation tools are available:
- **MCP servers** for testing
- **Error logs** the agent can monitor
- **Browser tools** for viewing UI elements
- **Test frameworks** for running automated tests
- **Git diff** for reviewing changes

The agent should loop through validation and testing before moving to the next task — catching errors early prevents cascading failures across subsequent tasks.

## Top 3 Tips

1. **90% planning, 10% execution** — Comprehensive Q&A, web research, and detailed planning before writing code → verify: step output matches expected outcome
2. **Agent-first project setup** — Context documents that let any new agent pick up where the last one left off → verify: step output matches expected outcome
3. **Agent-centric validation loops** — Discuss validation tools in planning so the agent knows how to verify its own work

## Meta-Engineering Your Workflow

Be critical of how you interact with Claude Code:
- Are you sending one-off prompts and going back and forth 100 times?
- Or giving structured prompts that enable autonomous completion?
- Ask Claude Code itself: "What skills or slash commands could make my workflow more efficient?"
- Ask Claude Code to interview YOU about your work to suggest global systems (hooks, skills, commands)

## When NOT to use

- Trivial single-file edits or one-line fixes — overhead exceeds value
- Pure exploration/research with no deliverable — use `researcher` or ad-hoc subagents
- Tight time-boxed spike where rework is expected and cheap
- Tasks where the requirements are already crystal-clear and short (just execute)
- Live incident response — use `incident-commander`, not multi-phase planning

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll skip discovery, requirements are obvious" | They never are; rework from missed intent costs more than 20 minutes of Q&A |
| "One huge plan file is fine, no sharding" | Subagents drown in 5k-line plans; shard or context-rot returns |
| "I'll track tasks in chat, no progress file" | Context compacts, you lose state; progress file is non-negotiable |
| "Validation comes after all phases done" | Errors compound; per-task validation catches them while cheap |

## Output Contract

Done when:
- `discovery.md` (or equivalent) captures Q&A verbatim with user
- `research.md` synthesizes parallel domain research
- `plan.md` has numbered phases, atomic tasks, dependencies, and per-task validation steps
- Tasks sharded into individual files under a task directory
- `progress.md` initialized as live source of truth for execution state
- Each task lists files-to-edit, validation tool, and atomic commit instruction
- User has explicitly approved the plan before execution starts

## Examples

### Example 1 — New feature: stripe checkout integration
- Input: "Add Stripe checkout to our Next.js app"
- Action: Run discovery Q&A (test vs live, products, webhooks?), parallel research (Stripe docs, sample repos, our auth flow), synthesize into 4-phase plan (config → server route → client UI → webhooks), shard into 8 tasks each with validation
- Output: `.agentic/discovery.md`, `.agentic/research.md`, `.agentic/plan.md`, `.agentic/tasks/phase-1/*.md`, `.agentic/progress.md` ready for subagent execution

### Example 2 — Resume after compaction
- Input: Fresh session opens mid-project, prior context gone
- Action: Read `progress.md` first, identify last completed task and next pending task, deploy executor subagent on the next task with its sharded brief
- Output: Execution continues without re-planning; main agent stays orchestrating, never re-fills its own context with implementation detail
