---
name: codex-adversarial-review
description: "Runs Codex CLI in adversarial auditor mode. Finds every mistake, violation, and poor decision Claude Code may have introduced. Triggers: 'use codex-adversarial-review', 'codex adversarial review', 'codex-adversarial-review task'."
allowed-tools: Bash, Glob, Grep, Read
---
Consolidated into `/codex` parent skill. For execution steps:
1. Read `~/.claude/skills/codex/SKILL.md` — mode routing + pre-check
2. Read `~/.claude/skills/codex/references/modes/adversarial-review.md` — adversarial audit steps
3. Read `~/.claude/skills/codex/references/common.md` — Red Flags + Output Contract
