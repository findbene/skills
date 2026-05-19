---
name: "adversarial-reviewer"
description: "Adversarial code review that breaks the self-review monoculture. Triggers: 'use adversarial-reviewer', 'adversarial reviewer', 'adversarial-reviewer task'."
allowed-tools: Glob, Grep, Read
tier: "STANDARD"
category: "Engineering / Code Quality"
dependencies: "None (prompt-only, no external tools required)"
author: "ekreloff"
version: "1.0.0"
license: "MIT"
---

# Adversarial Code Reviewer

## Examples

### Example: Reviewing a PR Before Merge

```
/adversarial-review --diff main...HEAD
```

Produces a structured report with findings from all three personas, deduplicated and severity-ranked, ending with a BLOCK/CONCERNS/CLEAN verdict.

## Review Workflow

### Step 1: Gather the Changes

Determine what to review based on invocation:

- **No arguments:** Run `git diff` (unstaged) + `git diff --cached` (staged). If both empty, run `git diff HEAD~1` (last commit).
- **`--diff <ref>`:** Run `git diff <ref>`.
- **`--file <path>`:** Read the entire file. Focus review on the full file rather than just changes.

If no changes are found, stop and report: "Nothing to review."

### Step 2: Read the Full Context

For every file in the diff:
1. Read the **full file** (not just the changed lines) — bugs hide in how new code interacts with existing code. → verify: file readable + content matches expected shape
2. Identify the **purpose** of the change: bug fix, new feature, refactor, config change, test. → verify: all tests pass
3. Note any **project conventions** from CLAUDE.md, .editorconfig, linting configs, or existing patterns. → verify: step output matches expected outcome

### Step 3: Run All Three Personas

Execute each persona sequentially. Each persona MUST produce at least one finding. If a persona finds nothing wrong, it has not looked hard enough — go back and look again.

**IMPORTANT:** Do not soften findings. Do not hedge. Do not say "this might be fine but..." — either it's a problem or it isn't. Be direct.

### Step 4: Deduplicate and Synthesize

After all three personas have reported:
1. Merge duplicate findings (same issue caught by multiple personas). → verify: step output matches expected outcome
2. Promote findings caught by 2+ personas to the next severity level. → verify: step output matches expected outcome
3. Produce the final structured output. → verify: step output matches expected outcome

## When to Use This

- **Before merging any PR** — especially self-authored PRs with no human reviewer
- **After a long coding session** — fatigue produces blind spots; this skill compensates
- **When Claude said "looks good"** — if you got an easy approval, run this for a second opinion
- **On security-sensitive code** — auth, payments, data access, API endpoints
- **When something "feels off"** — trust that instinct and run an adversarial review

## When NOT to use

- Task is unrelated to adversarial reviewer — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Adversarial Reviewer needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for adversarial reviewer
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## References

Extended sections moved to `references/details.md`.
