---
name: hooks-and-automation
description: "Claude Code hooks for event-driven automation, RALF loops, pre/post-tool actions, and workflow triggers within Claude Code sessions. Use this skill any time Claude Code hooks need to be configured, event-driven automation needs to be set up, pre or post-tool hooks need to be created, or automated workflows need to be triggered by Claude events. Trigger immediately on: \"Claude hooks\", \"hook\", \"pre-tool\", \"post-tool\", \"RALF\", \"event-driven\", \"automation hook\", \"Claude automation\", \"hooks configuration\", \"trigger on\", \"Claude event\", \"workflow trigger\", \"before tool\", \"after edit\". If someone says \"run this automatically when X happens\" or \"set up a Claude hook\" this skill MUST trigger."
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
1. Trigger separate instances of Claude Code SDK
2. Maintain external progress tracking documents
3. Each new instance checks the progress document
4. Loop continues until completion state is reached

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
