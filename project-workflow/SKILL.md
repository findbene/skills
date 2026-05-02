---
name: project-workflow
description: "Complete project lifecycle management covering discovery, planning, implementation tracking, and delivery workflows with 9 integrated commands. Use this skill any time a project needs lifecycle management, project workflows need to be structured, implementation tracking needs to be set up, or project delivery processes need to be defined. Trigger immediately on: \"project workflow\", \"project management\", \"project lifecycle\", \"project tracking\", \"project delivery\", \"sprint planning\", \"project phases\", \"project structure\", \"delivery workflow\", \"project organization\", \"manage this project\", \"agile workflow\", \"project setup\", \"project methodology\". If someone says \"set up project management\" or \"how do I manage this project?\" this skill MUST trigger."
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
