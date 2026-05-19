---
name: codex-explain
description: "Asks Codex CLI to deeply explain how a module, function, or system works. Triggers: 'use codex-explain', 'codex explain', 'codex-explain task'."
allowed-tools: Bash, Glob, Grep, Read
---
Consolidated into `/codex` parent skill. For execution steps:
1. Read `~/.claude/skills/codex/SKILL.md` — mode routing + pre-check
2. Read `~/.claude/skills/codex/references/modes/explain.md` — explain-specific steps
3. Read `~/.claude/skills/codex/references/common.md` — Red Flags + Output Contract
