---
name: ads-landing
description: "Landing page quality assessment for paid advertising campaigns. Evaluates message match, page speed, mobile experience, trust signals, and conversion optimization. Triggers: 'use ads-landing', 'run ads landing', 'ads landing'."
allowed-tools: Glob, Grep, Read
user-invokable: false
---
Consolidated into `/ads` parent skill. For execution steps:
1. Read `~/.claude/skills/ads/SKILL.md` — orchestration + context intake
2. Read `~/.claude/skills/ads/references/modes/landing.md` — landing-specific steps
