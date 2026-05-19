---
name: codex-refactor
description: "Delegates a focused refactor to Codex CLI. Use for structural improvements within a module — not new features. Triggers: 'use codex-refactor', 'codex refactor', 'codex-refactor task'."
allowed-tools: Bash, Glob, Grep, Read
---
Consolidated into `/codex` parent skill. For execution steps:
1. Read `~/.claude/skills/codex/SKILL.md` — mode routing + pre-check
2. Read `~/.claude/skills/codex/references/modes/refactor.md` — refactor-specific steps
3. Read `~/.claude/skills/codex/references/common.md` — Red Flags + Output Contract
