---
name: git-workflow
description: 'Git best practices for commits, branching, PRs, merge strategies, conflict resolution, and collaborative workflows. Triggers: "use git-workflow", "git workflow workflow", "git workflow".'
allowed-tools: Bash, Glob, Grep, Read
---

# Git Workflow Best Practices

Professional git patterns for teams and solo developers.

## Conventional Commits

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

### Types
| Type | Description |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation |
| `style` | Formatting (no code change) |
| `refactor` | Code change (no feature/fix) |
| `perf` | Performance improvement |
| `test` | Adding tests |
| `chore` | Build/tooling changes |

### Examples
```bash
feat(auth): add OAuth2 login support
fix(cart): resolve quantity update race condition
refactor(api): extract validation middleware
```

## Branch Strategy (GitHub Flow)

```
main ─────────────────────────────────────────►
       │                    │
       └── feature/auth ────┘ (PR + merge)
```

### Branch Naming
```bash
feature/user-authentication
fix/cart-quantity-bug
docs/api-documentation
refactor/database-queries
```

## Common Commands

```bash
# Start new feature
git checkout -b feature/new-feature

# Stage specific files
git add src/components/Button.tsx

# Commit with message
git commit -m "feat(ui): add Button component"

# Push and set upstream
git push -u origin feature/new-feature

# Rebase on main before PR
git fetch origin && git rebase origin/main

# Amend last commit
git commit --amend --no-edit

# Stash changes
git stash push -m "WIP: auth feature"
git stash pop
```

## Pull Request Best Practices

### PR Template
```markdown
## Summary
Brief description of changes

## Changes
- Added X
- Fixed Y

## Testing
- [ ] Unit tests pass
- [ ] Manual testing completed
```

## Conflict Resolution

```bash
# During rebase conflict
git status                    # See conflicting files
# Edit files to resolve
git add <resolved-files>
git rebase --continue

# Abort if needed
git rebase --abort
```

## Undo Mistakes

```bash
# Undo last commit (keep changes)
git reset --soft HEAD~1

# Revert a pushed commit (safe)
git revert <commit-hash>

# Restore deleted file
git checkout HEAD -- path/to/file
```

## Git Hooks (Husky)

```json
{
  "husky": { "hooks": { "pre-commit": "lint-staged", "commit-msg": "commitlint -E HUSKY_GIT_PARAMS" } },
  "lint-staged": { "*.{ts,tsx}": ["eslint --fix", "prettier --write"] }
}
```

## When NOT to use

- Task is unrelated to git workflow — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Git Workflow needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for git workflow
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving git workflow
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
