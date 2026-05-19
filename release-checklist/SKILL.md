---
name: release-checklist
description: 'Pre-release safety verification covering 8 phases including security scanning, performance testing, deployment readiness, and ro. Triggers: "use release-checklist", "release checklist", "release task.'
allowed-tools: Bash, Glob, Grep, Read
---

# Release Checklist

8-phase pre-release verification for safe deployments.

## Phase 1: Critical Safety
- No API keys, passwords, or private tokens in code
- .env not committed
- No personal paths, local usernames, or test credentials
- No debug endpoints
- Correct remote URL, clean branch, no uncommitted changes

## Phase 2: Documentation
- README.md exists and current
- LICENSE file present
- CHANGELOG.md updated
- CONTRIBUTING.md (if open source)

## Phase 3: Version & Changelog
- Version bumped in package.json (semver)
- Breaking changes = major bump
- Changelog entry with Added/Changed/Fixed sections

## Phase 4: Dependencies
- No unused dependencies
- No vulnerable packages (npm audit)
- Lock file committed
- Peer deps satisfied

## Phase 5: Quality Gates
- All tests passing, coverage meets threshold
- ESLint clean, TypeScript no errors
- Prettier formatted

## Phase 6: Build Verification
- Production build succeeds
- No build warnings
- Bundle size acceptable
- Assets optimized

## Phase 7: Deployment Prep
- ENV vars documented
- Secrets in vault
- Migrations ready, rollback tested, backup created

## Phase 8: Final Checks & Publish
- Git tag created
- Release notes written
- Team notified
- Publish and verify deployment
- Monitor for errors post-deploy

## Quick Checklist

```markdown
# Release: v[X.Y.Z]

## Critical
- [ ] Secrets scan passed
- [ ] Tests passing
- [ ] Build succeeds

## Required
- [ ] Version bumped
- [ ] Changelog updated
- [ ] README current

## Recommended
- [ ] Deps updated
- [ ] Docs reviewed
- [ ] Performance checked
```

## Automated Commands

```bash
npm run lint && npm run test && npm run build
npm version patch|minor|major
npm publish --access public
git tag -a v1.2.0 -m "Release v1.2.0"
git push origin v1.2.0
```

## When NOT to use

- Task is unrelated to release checklist — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Release Checklist needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for release checklist
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving release checklist
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
