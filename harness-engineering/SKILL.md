---
name: harness-engineering
description: 'Agent harness engineering for building reusable evaluation frameworks, test scaffolding, and skill validation workflows. Triggers: "use harness-engineering", "harness engineering", "harness task".'
allowed-tools: Glob, Grep, Read
---

# Harness Engineering

## What is a Harness?

**A harness** = All the tools, skills, context engineering workflows, and external documents that exist **outside** of the context window of your coding agent.

A harness allows you to start a brand new context window and have outputs be just as high-quality and project-specific as if you loaded all context manually.

## Three Core Components

### 1. Tool Configuration
- MCP servers for external integrations
- Browser automation tools
- Custom scripts for APIs and services
- Validation and testing tools

### 2. Skills Library
- Project-specific skills (coding patterns, architecture rules)
- Domain skills (business knowledge, industry patterns)
- Workflow skills (how to use specific tools correctly)
- Memory skills (storage, extraction, recall)

### 3. Context Documents
- `CLAUDE.md` — Project-level agent configuration
- `discovery.md` — Requirements and Q&A
- `research.md` — Domain research findings
- `plan.md` — Implementation roadmap
- `progress.md` — Current state and history
- Memory files — Long-term knowledge base

## Building a Reusable Harness

### Step 1: Identify Recurring Patterns
What do you do repeatedly? Testing workflows, deployment procedures, code review patterns, documentation generation.

### Step 2: Extract Into Skills
Turn each pattern into a skill file: when to apply, how to execute step by step, what tools are needed, how to validate results.

### Step 3: Create Slash Commands
Turn common workflows into quick-trigger commands: `/prime`, `/plan`, `/test`, `/review`, `/deploy`.

### Step 4: Configure Hooks
Event-driven automation: pre-compact memory saves, post-tool-use logging, session-start context loading.

### Step 5: Set Up Global Systems
Install in `~/.claude/` for cross-project benefit: global skills, hooks, slash commands, and CLAUDE.md.

## Harness vs Context Window

| Aspect | Context Window | Harness |
|--------|---------------|---------|
| Lifetime | One session | Permanent |
| Capacity | Limited tokens | Unlimited files |
| Recovery | Lost on compact | Always available |
| Reusability | None | Across all sessions |

## The Data Edge

Everyone uses the same models. Your competitive advantage is:
1. **Quality of your data** — Well-structured, comprehensive context → verify: step output matches expected outcome
2. **Shape of your data** — Agent-friendly formatting and organization → verify: step output matches expected outcome
3. **Domain-specific knowledge** — Your unique expertise encoded as skills → verify: step output matches expected outcome
4. **Consistency of collection** — Regular data gathering and organization → verify: step output matches expected outcome

## When NOT to use

- Task is unrelated to harness engineering — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Harness Engineering needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for harness engineering
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving harness engineering
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
