---
name: codex-diff
description: "Runs Codex against the current git diff before committing. Catches mistakes in the changes you're about to commit. Triggers: 'use codex-diff', 'codex diff', 'codex-diff task'."
allowed-tools: Bash, Glob, Grep, Read
---
Consolidated into `/codex` parent skill. For execution steps:
1. Read `~/.claude/skills/codex/SKILL.md` — mode routing + pre-check
2. Read `~/.claude/skills/codex/references/modes/diff.md` — diff-specific steps
3. Read `~/.claude/skills/codex/references/common.md` — Red Flags + Output Contract
