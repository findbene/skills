---
name: codex-fix
description: "Delegates a targeted fix to Codex CLI with full-auto approval. Use when you have a clear, specific issue to resolve and want Codex (o3) to implement it independently. Triggers: 'use codex-fix', 'codex fix', 'codex-fix task'."
allowed-tools: Bash, Glob, Grep, Read
---
Consolidated into `/codex` parent skill. For execution steps:
1. Read `~/.claude/skills/codex/SKILL.md` — mode routing + pre-check
2. Read `~/.claude/skills/codex/references/modes/fix.md` — fix-specific steps
3. Read `~/.claude/skills/codex/references/common.md` — Red Flags + Output Contract
