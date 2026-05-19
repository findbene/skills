---
name: codex-test
description: 'Delegates test writing to Codex CLI. Use when you need comprehensive tests for a module, feature, or after fixing a bug. Triggers: "use codex-test", "use codex test", "codex test".'
allowed-tools: Bash, Glob, Grep, Read
---
Consolidated into `/codex` parent skill. For execution steps:
1. Read `~/.claude/skills/codex/SKILL.md` — mode routing + pre-check
2. Read `~/.claude/skills/codex/references/modes/test.md` — test-specific steps
3. Read `~/.claude/skills/codex/references/common.md` — Red Flags + Output Contract
