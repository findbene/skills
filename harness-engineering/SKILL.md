---
name: harness-engineering
description: "Agent harness engineering for building reusable evaluation frameworks, test scaffolding, and skill validation workflows. Use this skill any time an agent eval harness needs to be built, skill test scaffolding needs to be created, or automated agent testing frameworks need to be designed. Trigger immediately on: \"eval harness\", \"agent harness\", \"skill evaluation\", \"test harness\", \"agent testing\", \"evaluation framework\", \"harness engineering\", \"skill benchmark\", \"agent eval\", \"test scaffold\", \"evaluation setup\", \"with_skill vs without_skill\", \"skill test\", \"harness setup\". If someone says \"build an eval harness\" or \"test this agent systematically\" this skill MUST trigger."
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
1. **Quality of your data** — Well-structured, comprehensive context
2. **Shape of your data** — Agent-friendly formatting and organization
3. **Domain-specific knowledge** — Your unique expertise encoded as skills
4. **Consistency of collection** — Regular data gathering and organization
