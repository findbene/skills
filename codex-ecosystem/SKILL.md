---
name: codex-ecosystem
description: 'Codex CLI configuration and MCP server management for initializing and managing OpenAI Codex in projects. Triggers: "use codex-ecosystem", "use codex ecosystem", "codex ecosystem".'
allowed-tools: Bash, Glob, Grep, Read
---
Consolidated into `/codex` parent skill. For execution steps:
1. Read `~/.claude/skills/codex/SKILL.md` — mode routing + pre-check
2. Read `~/.claude/skills/codex/references/modes/ecosystem.md` — bootstrap + MCP management steps
3. Read `~/.claude/skills/codex/references/common.md` — Red Flags + Output Contract
