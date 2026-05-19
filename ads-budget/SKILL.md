---
name: ads-budget
description: "Budget allocation and bidding strategy review across all ad platforms. Evaluates spend distribution, bidding strategy appropriateness, and scaling efficiency. Triggers: 'use ads-budget', 'run ads budget', 'ads budget'."
allowed-tools: Glob, Grep, Read
user-invokable: false
---
Consolidated into `/ads` parent skill. For execution steps:
1. Read `~/.claude/skills/ads/SKILL.md` — orchestration + context intake
2. Read `~/.claude/skills/ads/references/modes/budget.md` — budget-specific steps
