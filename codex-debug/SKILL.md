---
name: codex-debug
description: "Give Codex a specific error or failure to diagnose and fix. Best for tricky bugs that don't have an obvious cause. Triggers: 'use codex-debug', 'codex debug', 'codex-debug task'."
allowed-tools: Bash, Glob, Grep, Read
---
Consolidated into `/codex` parent skill. For execution steps:
1. Read `~/.claude/skills/codex/SKILL.md` — mode routing + pre-check
2. Read `~/.claude/skills/codex/references/modes/debug.md` — debug-specific steps
3. Read `~/.claude/skills/codex/references/common.md` — Red Flags + Output Contract
