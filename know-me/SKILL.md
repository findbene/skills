---
name: know-me
description: "Persistent user memory for surfacing and updating knowledge about the user, their preferences, context, and working style. Use this skill any time user context needs to be loaded, user preferences or history need to be referenced, or new information about the user needs to be saved to memory. Trigger immediately on: \"know me\", \"remember about me\", \"my preferences\", \"who am I\", \"load my profile\", \"user context\", \"personal context\", \"my background\", \"remember this about me\", \"update my profile\", \"user memory\", \"about me\", \"my working style\". If someone says \"remember that I prefer X\" or \"what do you know about me?\" this skill MUST trigger."
version: 1.0.0
triggers:
  - remember me
  - what do you know about me
  - load my context
  - who am I
  - my preferences
  - remember this about me
  - update my profile
  - know me
  - personal context
  - my background
  - I prefer
  - don't forget
references:
  - references/memory-operations.md
  - references/what-to-track.md
---

# Know-Me Skill

## Purpose
Build and maintain a persistent, accurate mental model of the user across sessions — so Claude provides personalized, context-aware assistance without the user repeating themselves.

## Activation
Load this skill when:
- User says "remember me", "what do you know about me", or "load my context"
- User corrects an assumption Claude made about them
- User shares a background detail or preference ("I prefer X", "I always do Y")
- Session start for a known user (load context proactively)
- User asks "do you remember...?"

## Memory Loading (Session Start)

Load from `C:\Users\findb\.claude\projects\C--AI-Agency\memory\`:
```
MEMORY.md          → index of all memory files
user_*.md          → user profile, role, preferences
feedback_*.md      → corrections and workflow preferences
project_*.md       → current project state
reference_*.md     → external resources and systems
```

After loading, produce a one-paragraph orientation:
> "Based on my memory: You're Biniyam Kebede, solopreneur building an autonomous AI agency.
> You have AWS/Azure/Power Platform certs, prefer step-by-step explanations, and are running
> 3 revenue streams. Current focus: [from progress.md]. Memory last updated: [date]."

## User Profile (Current — Biniyam Kebede)

- **Name**: Biniyam Kebede
- **Role**: Solopreneur, founder of Citadel AI (GitHub: findbene)
- **Technical level**: AWS CLP, Azure Fundamentals, MS Power Platform certified. Foundational knowledge — learning while building.
- **Communication preference**: Wants step-by-step explanations. Learns by doing.
- **Working style**: Mix of big sessions and small pushes. Pushes directly to main.
- **Default model**: Claude Sonnet 4.6
- **Project**: AI_Agency at C:\AI_Agency — 3-stream autonomous business
- **Fiance**: Lidia (wedding website at C:\Lidia)
- **Other projects**: SosinaMart (Next.js), AmazonAM (WordPress/WooCommerce), remotetechgear.com

## Memory Operations

### Adding a Memory
1. Determine type: `user` | `feedback` | `project` | `reference`
2. Write file to `C:\Users\findb\.claude\projects\C--AI-Agency\memory\{type}_{topic}.md`
3. Add pointer to MEMORY.md index: `- [type_topic]: one-line description`

### Updating a Memory
1. Search MEMORY.md index for existing file
2. Read it — update in place (never create duplicates)
3. Note date of update

### Forgetting a Memory
1. Find the relevant file
2. Remove the entry or delete the file
3. Update MEMORY.md index

## Personalization Rules

When memory is loaded, adjust responses:
- Explain at "foundational technical" level — not beginner, not senior engineer
- Use examples from his stack: Python, LangGraph, Supabase, n8n, Docker
- Don't over-explain git, Docker basics, or REST APIs
- DO explain LangGraph state management, Python async, cloud architecture patterns
- Reference AI Agency context when relevant
- Flag when something requires Biniyam's approval (spend > $20/month, client-facing output)
