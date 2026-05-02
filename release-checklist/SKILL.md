---
name: release-checklist
description: "Pre-release safety verification covering 8 phases including security scanning, performance testing, deployment readiness, and rollback planning. Use this skill any time software is about to be released, a deployment is being prepared, a pre-release checklist needs to be run, or release readiness needs to be verified. Trigger immediately on: \"release checklist\", \"pre-release\", \"ready to release\", \"deployment checklist\", \"release process\", \"launch checklist\", \"go live\", \"ready to deploy\", \"release verification\", \"release prep\", \"before we deploy\", \"launch prep\", \"production release\", \"ship it\". If someone says \"we are ready to release\" or \"prepare for deployment\" this skill MUST trigger."
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
