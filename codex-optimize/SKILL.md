---
name: codex-optimize
description: "Asks Codex to identify and fix performance bottlenecks in a target module. Triggers: 'use codex-optimize', 'codex optimize', 'codex-optimize task'."
allowed-tools: Bash, Glob, Grep, Read
---
Consolidated into `/codex` parent skill. For execution steps:
1. Read `~/.claude/skills/codex/SKILL.md` — mode routing + pre-check
2. Read `~/.claude/skills/codex/references/modes/optimize.md` — optimize-specific steps
3. Read `~/.claude/skills/codex/references/common.md` — Red Flags + Output Contract
