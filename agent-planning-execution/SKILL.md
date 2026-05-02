---
name: agent-planning-execution
description: "Structured planning and execution methodology for AI coding agents covering task decomposition, phase planning, and implementation tracking. Use this skill any time a complex coding task needs to be planned before execution, implementation phases need to be defined, or structured guidance on approaching a problem is required. Trigger immediately on: \"plan this\", \"implementation plan\", \"task decomposition\", \"how should I approach\", \"break this down\", \"where do I start\", \"planning phase\", \"execution strategy\", \"roadmap\", \"agent plan\", \"step by step approach\", \"structured plan\", \"phases\". If someone says \"how should I tackle this?\" or \"plan out this feature\" this skill MUST trigger."
---

# Agent Planning & Execution

A context engineering system that prevents **context rot** — the quality degradation that happens as Claude fills its context window.

## Core Concept

Segment your project into tasks, each accomplished by a subagent with its own fresh context window. This keeps every execution context clean and prevents the cumulative quality loss that comes from overloaded context windows.

### Key Mechanisms
1. **Fresh 200k contexts per task** — Each executor runs in a clean context
2. **Persistent project memory** — State lives in markdown files, not chat history
3. **Parallel orchestration** — Work happens in subagents; main session stays responsive
4. **Atomic tasks** — Each task is self-contained with explicit verification
5. **Atomic Git commits** — Each completed task committed separately for history/rollback

## Phased Execution Workflow

### Phase 1: Research & Discovery
1. Ask extremely detailed, pointed questions about the project
2. Conduct comprehensive deep research (web search, GitHub implementations, documentation)
3. Store all findings in external research documents

The agent often has better domain knowledge than the user — thorough questioning surfaces considerations the user might miss.

### Phase 2: Planning
1. Review all Q&A and research documents
2. Synthesize into a phased plan
3. Consider the agent's environment: available tools, validation methods, testing capabilities
4. Create one source of truth document with all phases and subtasks
5. Reference exact Q&A and research documents for each task

### Phase 3: Task Sharding
Break the plan into individual subtask documents organized in a task directory with phase subdirectories. Each task file includes:
- How this subtask fits the larger plan (short summary)
- Specific implementation steps
- Files to create/edit
- Connection to previously completed and future subtasks
- Validation steps using available tools
- Atomic git commit instruction

### Phase 4: Orchestrated Execution
1. Configure progress tracking file (summary of all subtasks)
2. Main agent deploys subagents for each subtask sequentially
3. Subagents mark completed tasks in progress file
4. If main context compacts, read progress file to resume
5. Main context stays light (only orchestration overhead)

## Agent-Centric Validation

During planning, explicitly discuss what validation tools are available:
- **MCP servers** for testing
- **Error logs** the agent can monitor
- **Browser tools** for viewing UI elements
- **Test frameworks** for running automated tests
- **Git diff** for reviewing changes

The agent should loop through validation and testing before moving to the next task — catching errors early prevents cascading failures across subsequent tasks.

## Top 3 Tips

1. **90% planning, 10% execution** — Comprehensive Q&A, web research, and detailed planning before writing code
2. **Agent-first project setup** — Context documents that let any new agent pick up where the last one left off
3. **Agent-centric validation loops** — Discuss validation tools in planning so the agent knows how to verify its own work

## Meta-Engineering Your Workflow

Be critical of how you interact with Claude Code:
- Are you sending one-off prompts and going back and forth 100 times?
- Or giving structured prompts that enable autonomous completion?
- Ask Claude Code itself: "What skills or slash commands could make my workflow more efficient?"
- Ask Claude Code to interview YOU about your work to suggest global systems (hooks, skills, commands)
