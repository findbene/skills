---
name: hooks-and-automation
description: "Claude Code hooks for event-driven automation, RALF loops, pre/post-tool actions, and workflow triggers within Claude Code sessions."
allowed-tools: Glob, Grep, Read
---

# Hooks & Automation

## Claude Code Hooks

Hooks trigger scripts based on events in Claude Code's operation cycle.

### Hook Types
- **Pre-tool-use** — Before Claude executes a tool
- **Post-tool-use** — After a tool execution completes
- **Pre-compact** — Before context window compaction
- **Post-compact** — After compaction
- **Session hooks** — On session start/end

### Key Hook Patterns

#### Memory Extraction Hook (Pre-Compact)
Trigger at ~80% context usage. Prompt Claude to save memories before compaction — deploy background subagent to read transcript, extract key insights, store in persistent memory.

#### Continual Learning Hook
Trigger at end of every session. Force Claude to extract key learnings, distill into insights, store in memory database, commit to Git for time-tracking.

### Hookify Plugin
Instead of manually editing `settings.json`, use **hookify** — gives Claude Code skills to configure hooks accurately on first attempt. Install from the plugin marketplace.

## RALF Loops

RALF (Repeat And Loop Forever) loops re-inject prompts into Claude Code's context to programmatically execute tasks until completion.

### Correct Implementation
Use **Claude Code SDK** (not the CLI plugin) to create loops that:
1. Trigger separate instances of Claude Code SDK → verify: step output matches expected outcome
2. Maintain external progress tracking documents → verify: step output matches expected outcome
3. Each new instance checks the progress document → verify: step output matches expected outcome
4. Loop continues until completion state is reached → verify: step output matches expected outcome

### RALF + GSD Together
- **GSD** = Turns ideas into structured task documents/prompts
- **RALF** = Mechanism to programmatically execute tasks in sequence

Combined: GSD structures tasks, RALF executes them until all tests pass.

## n8n Integration

### n8n MCP Server
Gives Claude Code direct access to your n8n instance: deploy, edit, validate workflows, access 2,800+ templates.

### n8n Skills Repository
Seven skills teaching Claude Code how to use the n8n MCP server correctly, navigate tools, and build validated workflows from natural language prompts.

## NotebookLM MCP Server

Use NotebookLM to massively expand research context — compile millions of tokens of research in ~5 minutes, import via MCP server, interact with more context than fits in one window.

## When NOT to use

- Task doesn't involve configuring Claude Code hooks and automation → use the matching domain skill instead
- Simple one-off operation that doesn't need this skill's structure
- Different toolchain required → check `find-skills` skill for alternatives
- User explicitly asks to skip skill discipline → respect the override

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll skip the verify step, output looks right" | Eyeballing without verification ships broken outputs |
| "Generic answer is good enough" | hook configuration needs domain-specific decisions, not boilerplate |
| "I can hold all context in head" | Multi-step state loss creates regressions you won't catch |
| "Just one more shortcut" | Shortcuts compound — finish discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced and matches user's stated goal
- All verification steps in process passed
- Edge cases for configuring Claude Code hooks and automation addressed or explicitly noted
- Output is reproducible (no hidden state)
- Hand-off summary provided so user can validate without re-reading entire flow

## Examples

### Example 1 — golden path
- Input: standard request involving configuring Claude Code hooks and automation
- Action: follow the documented numbered process, apply verify clauses per step
- Output: deliverable that passes the Output Contract

### Example 2 — edge case
- Input: request with non-standard constraint or partial info
- Action: detect the gap, ask clarifying question OR document assumption, proceed with adapted process
- Output: deliverable + explicit note on assumption/limitation
