---
name: ads-competitor
description: "Competitor ad intelligence analysis across Google, Meta, LinkedIn, TikTok, Microsoft, and Apple Ads. Triggers: 'use ads-competitor', 'run ads competitor', 'ads competitor'."
allowed-tools: Glob, Grep, Read
user-invokable: false
---
Consolidated into `/ads` parent skill. For execution steps:
1. Read `~/.claude/skills/ads/SKILL.md` — orchestration + context intake
2. Read `~/.claude/skills/ads/references/modes/competitor.md` — competitor-specific steps
