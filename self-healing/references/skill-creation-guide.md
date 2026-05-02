# Skill Creation Guide

## When to Create a New Skill

Create when:
1. You encounter the same type of problem 3+ times across sessions
2. A domain requires substantial reference knowledge (> 500 tokens)
3. A workflow has 5+ steps that should always run in order
4. You need to prime a subagent with specialized context

## Skill 2.0 Format

```markdown
---
name: skill-name                 # kebab-case, matches directory name
description: One-sentence        # used for triggering decision
version: 1.0.0
triggers:
  - trigger phrase 1
  - trigger phrase 2
references:                      # optional
  - references/filename.md
---

# Skill Title

## Purpose
[2-3 sentences: what it does and why it exists]

## Activation
Load when user asks about:
- [scenario 1]
- [scenario 2]

## [Main content]

## Key Principles
[3-7 bullet points: the most critical rules/decisions]
```

## Reference File Format
```markdown
# Reference Title

## Section 1
[Content with working code examples]

## Section 2
[More reference content]
```

Rules:
- Each file = ONE focused topic
- Always include working code examples (not pseudocode)
- Use headers so content is scannable
- Include failure modes alongside solutions

## Installing a Skill

```bash
# 1. Create directories (if not already present)
mkdir C:\Users\findb\.claude\skills\my-skill
mkdir C:\Users\findb\.claude\skills\my-skill\references

# 2. Write SKILL.md
# 3. Write reference files (optional)
# 4. Update MEMORY.md index:
# Append: "- **my-skill**: [description]"
```

## On-the-Fly Skill Creation

When you notice a pattern worth preserving:
```
1. Identify: "This is the 3rd time we've solved X the same way"
2. Create: Write SKILL.md + reference files
3. Document: Add to MEMORY.md index
4. Test: Reference the skill in a follow-up
```

Trigger phrases:
- "We keep hitting ElevenLabs timeout errors — creating elevenlabs-debug skill"
- "This LangGraph state pattern keeps coming up — creating langgraph-patterns skill"

## Skill Quality Checklist

Good skill:
- [ ] Clear trigger set (not too narrow, not too broad)
- [ ] Explains WHEN to use it (not just how)
- [ ] Working code examples (not pseudocode)
- [ ] Lists failure modes alongside solutions
- [ ] Domain-specific (not "python best practices" — too broad)
- [ ] References for detail; SKILL.md for synthesis

Bad skill:
- Duplicates an existing skill (check MEMORY.md first)
- Vague triggers ("when coding")
- Too general to be actionable
- Only links, no synthesized guidance
- Longer than needed to convey the workflow
