# What to Track in User Memory

## User Type Memories

### Role & Identity
- Job title and primary responsibilities
- Company/project names and their purposes
- Team size (solo, small team, enterprise)
- Industry and domain expertise
- Years of experience in relevant areas
- Key certifications and formal qualifications

### Technical Profile
- Primary programming languages (with proficiency level)
- Preferred frameworks and tools
- Cloud providers in use
- Operating system and dev environment
- Known gaps (areas needing more explanation)
- Areas of deep expertise (less explanation needed)

### Communication Preferences
- Level of detail desired (high-level vs. step-by-step)
- Preferred format (bullets vs. prose, code vs. explanation)
- Terminology preferences (specific jargon they use)

### Working Style
- Solo vs. collaborative
- Git workflow (direct push, PRs, branch strategy)
- Session patterns (short/frequent vs. long/infrequent)

---

## Feedback Type Memories

The most critical memories — Claude repeating corrected behavior frustrates users.

Examples to capture:
- "Don't add emojis to code comments"
- "Don't summarize what you just did at the end"
- "Always read the file before suggesting edits"
- "Don't create separate type files — keep them inline"
- "Push directly to main, don't ask about PRs every time"

### Structure for Each Feedback Memory
```markdown
**Rule**: [What to do / not do]
**Why**: [Reason given]
**How to apply**: [When this triggers]
```

---

## Project Type Memories

### Current State
- What's being worked on right now
- Recent completed milestones
- Blockers and open questions
- Next planned tasks

### Key Decisions
- Architecture choices and rationale
- Tools chosen (and what was rejected + why)
- Constraints affecting future decisions

### Stakeholders
- Who approves what (Biniyam approves all client-facing + spend > $20/mo)
- External dependencies (clients, APIs)

---

## Reference Type Memories

- URLs for important dashboards, docs, APIs
- Where specific types of information live
- Which documentation is most reliable
- Internal docs that answer common questions

---

## What NOT to Track

- Session-specific state (temp file paths, debug output)
- Information already in CLAUDE.md or progress.md
- Speculative conclusions from one file
- Credentials or actual secret values
- Negative judgments about the user
- Information the user hasn't confirmed
