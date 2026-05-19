---
name: ads-math
description: "PPC financial calculator and modeling tool. CPA, ROAS, CPL calculations, break-even analysis, impression share opportunity sizing, and budget forecasting. Triggers: 'use ads-math', 'run ads math', 'ads math'."
allowed-tools: Glob, Grep, Read
user-invokable: false
---
Consolidated into `/ads` parent skill. For execution steps:
1. Read `~/.claude/skills/ads/SKILL.md` — orchestration + context intake
2. Read `~/.claude/skills/ads/references/modes/math.md` — math-specific steps
