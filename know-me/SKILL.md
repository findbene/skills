---
name: know-me
description: "Persistent user memory for surfacing and updating knowledge about the user, their preferences, context, and working style. Triggers: 'use know-me', 'know me', 'know-me task'."
allowed-tools: Glob, Grep, Read
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
1. Determine type: `user` | `feedback` | `project` | `reference` → verify: step output matches expected outcome
2. Write file to `C:\Users\findb\.claude\projects\C--AI-Agency\memory\{type}_{topic}.md` → verify: output exists + parses without error
3. Add pointer to MEMORY.md index: `- [type_topic]: one-line description` → verify: dependency resolves + import works

### Updating a Memory
1. Search MEMORY.md index for existing file → verify: step output matches expected outcome
2. Read it — update in place (never create duplicates) → verify: file content matches expected shape
3. Note date of update → verify: step output matches expected outcome

### Forgetting a Memory
1. Find the relevant file → verify: step output matches expected outcome
2. Remove the entry or delete the file → verify: step output matches expected outcome
3. Update MEMORY.md index → verify: step output matches expected outcome

## Personalization Rules

When memory is loaded, adjust responses:
- Explain at "foundational technical" level — not beginner, not senior engineer
- Use examples from his stack: Python, LangGraph, Supabase, n8n, Docker
- Don't over-explain git, Docker basics, or REST APIs
- DO explain LangGraph state management, Python async, cloud architecture patterns
- Reference AI Agency context when relevant
- Flag when something requires Biniyam's approval (spend > $20/month, client-facing output)

## Triggers

\\\"know me\\\", \\\"remember about me\\\", \\\"my preferences\\\", \\\"who am I\\\", \\\"load my profile\\\", \\\"user context\\\", \\\"personal context\\\", \\\"my background\\\", \\\"remember this about me\\\", \\\"update my profile\\\", \\\"user memory\\...

## When NOT to use

- Task is unrelated to know me — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Know Me needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for know me
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving know me
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
