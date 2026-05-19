---
name: codex-security
description: "Runs Codex CLI in security-specialist mode. Focuses exclusively on vulnerabilities, secrets exposure, auth bypass, injection vectors, and tenant isolation gaps. Triggers: 'use codex-security', 'codex security', 'codex-security task'."
allowed-tools: Bash, Glob, Grep, Read
---
Consolidated into `/codex` parent skill. For execution steps:
1. Read `~/.claude/skills/codex/SKILL.md` — mode routing + pre-check
2. Read `~/.claude/skills/codex/references/modes/security.md` — security audit steps + custom Red Flags
3. Read `~/.claude/skills/codex/references/common.md` — shared Output Contract
