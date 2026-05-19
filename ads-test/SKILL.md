---
name: ads-test
description: "A/B test design and experiment planning for paid advertising. Triggers: 'use ads-test', 'run ads test', 'ads test'."
allowed-tools: Glob, Grep, Read
user-invokable: false
---
Consolidated into `/ads` parent skill. For execution steps:
1. Read `~/.claude/skills/ads/SKILL.md` — orchestration + context intake
2. Read `~/.claude/skills/ads/references/modes/test.md` — test-specific steps
