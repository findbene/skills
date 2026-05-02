---
name: git-workflow
description: "Git best practices for commits, branching, PRs, merge strategies, conflict resolution, and collaborative workflows. Use this skill any time git operations need guidance, commit messages need to be written, branches need to be created, PRs need to be structured, or git conflicts need to be resolved. Trigger immediately on: \"git commit\", \"commit message\", \"branch\", \"pull request\", \"PR\", \"merge\", \"rebase\", \"git conflict\", \"conventional commit\", \"git workflow\", \"git history\", \"cherry pick\", \"git tag\", \"undo commit\". If someone asks about git operations or needs help with a commit message this skill MUST trigger."
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
