---
name: codex-integration
description: 'Delegate complex coding tasks to OpenAI Codex CLI and coordinate Codex with Claude Code in multi-agent workflows. Triggers: "use codex-integration", "use codex integration", "codex integration".'
allowed-tools: Glob, Grep, Read
---
Consolidated into `/codex` parent skill. For execution steps:
1. Read `~/.claude/skills/codex/SKILL.md` — mode routing + pre-check
2. Read `~/.claude/skills/codex/references/modes/integration.md` — delegation patterns + mode selection
3. Read `~/.claude/skills/codex/references/common.md` — Red Flags + Output Contract
