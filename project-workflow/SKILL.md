---
name: project-workflow
description: 'Complete project lifecycle management covering discovery, planning, implementation tracking, and delivery workflows with 9 integra. Triggers: "use project-workflow", "project workflow", "project task.'
allowed-tools: Glob, Grep, Read
---

# Project Workflow

Complete project lifecycle management — saves 35-55 minutes per lifecycle.

## The 9 Commands

### 1. /explore-idea — Pre-Planning
**When:** Rough ideas needing validation
**Creates:** PROJECT_BRIEF.md with validated decisions

### 2. /plan-project — Generate Planning Docs
**When:** Starting new projects
**Creates:** IMPLEMENTATION_PHASES.md, SESSION.md, DATABASE_SCHEMA.md, API_ENDPOINTS.md, ARCHITECTURE.md

### 3. /plan-feature — Add Features
**When:** Adding to existing projects
**Does:** Generate phases, integrate into IMPLEMENTATION_PHASES.md, update SESSION.md

### 4. /wrap-session — End Session Checkpoint
**When:** Context full (>150k tokens), end of work, task switching
**Does:** Update SESSION.md (progress, next action, blockers), git checkpoint commit

### 5. /continue-session — Resume Work
**When:** Starting new session
**Does:** Load SESSION.md + planning docs, show git history, open relevant files

### 6. /workflow — Interactive Guide
**When:** Uncertain which command to use
**Does:** Context-aware guidance and decision trees

### 7. /release — Pre-Release Checks
**When:** Ready to publish
**8 Phases:** Secrets scan, personal artifacts, git verification, LICENSE, README, CONTRIBUTING.md, quality checks, auto-fix

### 8. /brief — Context Preservation
**When:** Before clearing context
**Creates:** docs/brief-[slug].md with extracted decisions

### 9. /reflect — Capture Learnings
**When:** After significant work
**Does:** Identify workflows, patterns, suggest custom agents

## Workflow Patterns

### Full Lifecycle
```
/explore-idea → /plan-project → work → /wrap-session
→ /continue-session → /plan-feature → /reflect → /release
```

### Quick Start
```
/plan-project → work → /wrap-session → /continue-session → /release
```

## Key Files

| File | Purpose |
|------|---------|
| SESSION.md | Current progress, next actions, blockers |
| IMPLEMENTATION_PHASES.md | Phased development plan |
| PROJECT_BRIEF.md | Validated requirements |
| ARCHITECTURE.md | System design |
| API_ENDPOINTS.md | API contracts |
| DATABASE_SCHEMA.md | Data model |

## When NOT to use

- Task is unrelated to project workflow — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Project Workflow needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for project workflow
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving project workflow
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
