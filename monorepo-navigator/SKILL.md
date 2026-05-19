---
name: "monorepo-navigator"
description: 'Navigate, analyze, and optimize monorepos using Turborepo, Nx, pnpm workspaces, or Lerna - cross-package impact a. Triggers: ''use monorepo-navigator'', ''monorepo navigator'', ''monorepo-navigator task.'
allowed-tools: Bash, Glob, Grep, Read
---

## Overview

Navigate, manage, and optimize monorepos. Covers Turborepo, Nx, pnpm workspaces, and Lerna. Enables cross-package impact analysis, selective builds/tests on affected packages only, remote caching, dependency graph visualization, and structured migrations from multi-repo to monorepo. Includes Claude Code configuration for workspace-aware development.

---

## Core Capabilities

- **Cross-package impact analysis** — determine which apps break when a shared package changes
- **Selective commands** — run tests/builds only for affected packages (not everything)
- **Dependency graph** — visualize package relationships as Mermaid diagrams
- **Build optimization** — remote caching, incremental builds, parallel execution
- **Migration** — step-by-step multi-repo → monorepo with zero history loss
- **Publishing** — changesets for versioning, pre-release channels, npm publish workflows
- **Claude Code config** — workspace-aware CLAUDE.md with per-package instructions

---

## When to Use

Use when:
- Multiple packages/apps share code (UI components, utils, types, API clients)
- Build times are slow because everything rebuilds when anything changes
- Migrating from multiple repos to a single repo
- Need to publish packages to npm with coordinated versioning
- Teams work across multiple packages and need unified tooling

Skip when:
- Single-app project with no shared packages
- Team/project boundaries are completely isolated (polyrepo is fine)
- Shared code is minimal and copy-paste overhead is acceptable

---

## Tool Selection

| Tool | Best For | Key Feature |
|---|---|---|
| **Turborepo** | JS/TS monorepos, simple pipeline config | Best-in-class remote caching, minimal config |
| **Nx** | Large enterprises, plugin ecosystem | Project graph, code generation, affected commands |
| **pnpm workspaces** | Workspace protocol, disk efficiency | `workspace:*` for local package refs |
| **Lerna** | npm publishing, versioning | Batch publishing, conventional commits |
| **Changesets** | Modern versioning (preferred over Lerna) | Changelog generation, pre-release channels |

Most modern setups: **pnpm workspaces + Turborepo + Changesets**

---

## Turborepo
→ See references/monorepo-tooling-reference.md for details

## Workspace Analyzer

```bash
python3 scripts/monorepo_analyzer.py /path/to/monorepo
python3 scripts/monorepo_analyzer.py /path/to/monorepo --json
```

Also see `references/monorepo-patterns.md` for common architecture and CI patterns.

## Common Pitfalls

| Pitfall | Fix |
|---|---|
| Running `turbo run build` without `--filter` on every PR | Always use `--filter=...[origin/main]` in CI |
| `workspace:*` refs cause publish failures | Use `pnpm changeset publish` — it replaces `workspace:*` with real versions automatically |
| All packages rebuild when unrelated file changes | Tune `inputs` in turbo.json to exclude docs, config files from cache keys |
| Shared tsconfig causes one package to break all type-checks | Use `extends` properly — each package extends root but overrides `rootDir` / `outDir` |
| git history lost during migration | Use `git filter-repo --to-subdirectory-filter` before merging — never move files manually |
| Remote cache not working in CI | Check TURBO_TOKEN and TURBO_TEAM env vars; verify with `turbo run build --summarize` |
| CLAUDE.md too generic — Claude modifies wrong package | Add explicit "When working on X, only touch files in apps/X" rules per package CLAUDE.md |

---

## Best Practices

1. **Root CLAUDE.md defines the map** — document every package, its purpose, and dependency rules → verify: step output matches expected outcome
2. **Per-package CLAUDE.md defines the rules** — what's allowed, what's forbidden, testing commands → verify: all checks pass
3. **Always scope commands with --filter** — running everything on every change defeats the purpose → verify: command exit code 0
4. **Remote cache is not optional** — without it, monorepo CI is slower than multi-repo CI → verify: step output matches expected outcome
5. **Changesets over manual versioning** — never hand-edit package.json versions in a monorepo → verify: diff matches intent
6. **Shared configs in root, extended in packages** — tsconfig.base.json, .eslintrc.base.js, jest.base.config.js → verify: step output matches expected outcome
7. **Impact analysis before merging shared package changes** — run affected check, communicate blast radius → verify: command exit code 0
8. **Keep packages/types as pure TypeScript** — no runtime code, no dependencies, fast to build and type-check → verify: command exit code 0

## When NOT to use

- Task doesn't involve navigating monorepo codebases → use the matching domain skill instead
- Simple one-off operation that doesn't need this skill's structure
- Different toolchain required → check `find-skills` skill for alternatives
- User explicitly asks to skip skill discipline → respect the override

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll skip the verify step, output looks right" | Eyeballing without verification ships broken outputs |
| "Generic answer is good enough" | monorepo navigation needs domain-specific decisions, not boilerplate |
| "I can hold all context in head" | Multi-step state loss creates regressions you won't catch |
| "Just one more shortcut" | Shortcuts compound — finish discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced and matches user's stated goal
- All verification steps in process passed
- Edge cases for navigating monorepo codebases addressed or explicitly noted
- Output is reproducible (no hidden state)
- Hand-off summary provided so user can validate without re-reading entire flow

## Examples

### Example 1 — golden path
- Input: standard request involving navigating monorepo codebases
- Action: follow the documented numbered process, apply verify clauses per step
- Output: deliverable that passes the Output Contract

### Example 2 — edge case
- Input: request with non-standard constraint or partial info
- Action: detect the gap, ask clarifying question OR document assumption, proceed with adapted process
- Output: deliverable + explicit note on assumption/limitation
