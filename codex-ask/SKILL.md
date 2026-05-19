---
name: codex-ask
description: 'Free-form question interface to Codex. Use when you want a second opinion, explore a decision, or need Codex to investigate something specific. Triggers: "use codex-ask", "use codex ask", "codex ask".'
allowed-tools: Bash, Glob, Grep, Read
---
Consolidated into `/codex` parent skill. For execution steps:
1. Read `~/.claude/skills/codex/SKILL.md` — mode routing + pre-check
2. Read `~/.claude/skills/codex/references/modes/ask.md` — ask-specific steps
3. Read `~/.claude/skills/codex/references/common.md` — Red Flags + Output Contract
