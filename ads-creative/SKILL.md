---
name: ads-creative
description: "Cross-platform creative quality audit covering ad copy, video, image, and format diversity across all platforms. Triggers: 'use ads-creative', 'run ads creative', 'ads creative'."
allowed-tools: Glob, Grep, Read
user-invokable: false
---
Consolidated into `/ads` parent skill. For execution steps:
1. Read `~/.claude/skills/ads/SKILL.md` — orchestration + context intake
2. Read `~/.claude/skills/ads/references/modes/creative.md` — creative-specific steps
