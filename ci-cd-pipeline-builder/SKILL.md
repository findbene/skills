---
name: ci-cd-pipeline-builder
description: 'CI/CD Pipeline Builder. Triggers: "use ci-cd-pipeline-builder", "ci cd pipeline builder", "ci task".'
allowed-tools: Bash, Glob, Grep, Read
---
---
name: "ci-cd-pipeline-builder"
description: "Generate CI/CD pipelines from project stack signals - GitHub Actions, GitLab CI, CircleCI, or Jenkins - with lint, test, build, and environment-aware deploy stages. Use when setting up CI/CD from scratch, fixing broken pipelines, or adding new stages. Trigger on: 'CI/CD pipeline', 'GitHub Actions', 'GitLab CI', 'pipeline setup', 'automate builds', 'deploy pipeline', 'continuous integration', 'continuous deployment', 'build automation', 'CI pipeline'."

# CI/CD Pipeline Builder

**Tier:** POWERFUL  
**Category:** Engineering  
**Domain:** DevOps / Automation

## Overview

Use this skill to generate pragmatic CI/CD pipelines from detected project stack signals, not guesswork. It focuses on fast baseline generation, repeatable checks, and environment-aware deployment stages.

## Core Capabilities

- Detect language/runtime/tooling from repository files
- Recommend CI stages (`lint`, `test`, `build`, `deploy`)
- Generate GitHub Actions or GitLab CI starter pipelines
- Include caching and matrix strategy based on detected stack
- Emit machine-readable detection output for automation
- Keep pipeline logic aligned with project lockfiles and build commands

## When to Use

- Bootstrapping CI for a new repository
- Replacing brittle copied pipeline files
- Migrating between GitHub Actions and GitLab CI
- Auditing whether pipeline steps match actual stack
- Creating a reproducible baseline before custom hardening

## Key Workflows

### 1. Detect Stack

```bash
python3 scripts/stack_detector.py --repo . --format text
python3 scripts/stack_detector.py --repo . --format json > detected-stack.json
```

Supports input via stdin or `--input` file for offline analysis payloads.

### 2. Generate Pipeline From Detection

```bash
python3 scripts/pipeline_generator.py \
  --input detected-stack.json \
  --platform github \
  --output .github/workflows/ci.yml \
  --format text
```

Or end-to-end from repo directly:

```bash
python3 scripts/pipeline_generator.py --repo . --platform gitlab --output .gitlab-ci.yml
```

### 3. Validate Before Merge

1. Confirm commands exist in project (`test`, `lint`, `build`). → verify: all checks pass
2. Run generated pipeline locally where possible. → verify: output exists + parses without error
3. Ensure required secrets/env vars are documented. → verify: step output matches expected outcome
4. Keep deploy jobs gated by protected branches/environments. → verify: step output matches expected outcome

### 4. Add Deployment Stages Safely

- Start with CI-only (`lint/test/build`).
- Add staging deploy with explicit environment context.
- Add production deploy with manual gate/approval.
- Keep rollout/rollback commands explicit and auditable.

## Script Interfaces

- `python3 scripts/stack_detector.py --help`
  - Detects stack signals from repository files
  - Reads optional JSON input from stdin/`--input`
- `python3 scripts/pipeline_generator.py --help`
  - Generates GitHub/GitLab YAML from detection payload
  - Writes to stdout or `--output`

## Common Pitfalls

1. Copying a Node pipeline into Python/Go repos → verify: dependency resolves + import works
2. Enabling deploy jobs before stable tests → verify: all checks pass
3. Forgetting dependency cache keys → verify: step output matches expected outcome
4. Running expensive matrix builds for every trivial branch → verify: command exit code 0
5. Missing branch protections around prod deploy jobs → verify: step output matches expected outcome
6. Hardcoding secrets in YAML instead of CI secret stores → verify: step output matches expected outcome

## Best Practices

1. Detect stack first, then generate pipeline. → verify: output exists + parses without error
2. Keep generated baseline under version control. → verify: output exists + parses without error
3. Add one optimization at a time (cache, matrix, split jobs). → verify: dependency resolves + import works
4. Require green CI before deployment jobs. → verify: step output matches expected outcome
5. Use protected environments for production credentials. → verify: step output matches expected outcome
6. Regenerate pipeline when stack changes significantly. → verify: output exists + parses without error

## References

- [references/github-actions-templates.md](references/github-actions-templates.md)
- [references/gitlab-ci-templates.md](references/gitlab-ci-templates.md)
- [references/deployment-gates.md](references/deployment-gates.md)
- [README.md](README.md)

## Detection Heuristics

The stack detector prioritizes deterministic file signals over heuristics:

- Lockfiles determine package manager preference
- Language manifests determine runtime families
- Script commands (if present) drive lint/test/build commands
- Missing scripts trigger conservative placeholder commands

## Generation Strategy

Start with a minimal, reliable pipeline:

1. Checkout and setup runtime → verify: command exit code 0
2. Install dependencies with cache strategy → verify: dependency resolves + import works
3. Run lint, test, build in separate steps → verify: command exit code 0
4. Publish artifacts only after passing checks → verify: step output matches expected outcome

Then layer advanced behavior (matrix builds, security scans, deploy gates).

## Platform Decision Notes

- GitHub Actions for tight GitHub ecosystem integration
- GitLab CI for integrated SCM + CI in self-hosted environments
- Keep one canonical pipeline source per repo to reduce drift

## Validation Checklist

1. Generated YAML parses successfully. → verify: output exists + parses without error
2. All referenced commands exist in the repo. → verify: step output matches expected outcome
3. Cache strategy matches package manager. → verify: step output matches expected outcome
4. Required secrets are documented, not embedded. → verify: step output matches expected outcome
5. Branch/protected-environment rules match org policy. → verify: step output matches expected outcome

## Scaling Guidance

- Split long jobs by stage when runtime exceeds 10 minutes.
- Introduce test matrix only when compatibility truly requires it.
- Separate deploy jobs from CI jobs to keep feedback fast.
- Track pipeline duration and flakiness as first-class metrics.

## When NOT to use

- Task is unrelated to ci cd pipeline builder — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Ci Cd Pipeline Builder needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for ci cd pipeline builder
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving ci cd pipeline builder
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
