---
name: "release-manager"
description: 'Use when the user asks to plan releases, manage changelogs, coordinate deployments, create release branches, or automate versioning. Triggers: "use release-manager", "release manager", "release task".'
allowed-tools: Glob, Grep, Read
---

# Release Manager

**Tier:** POWERFUL  
**Category:** Engineering  
**Domain:** Software Release Management & DevOps

## Overview

The Release Manager skill provides comprehensive tools and knowledge for managing software releases end-to-end. From parsing conventional commits to generating changelogs, determining version bumps, and orchestrating release processes, this skill ensures reliable, predictable, and well-documented software releases.

## Core Capabilities

- **Automated Changelog Generation** from git history using conventional commits
- **Semantic Version Bumping** based on commit analysis and breaking changes
- **Release Readiness Assessment** with comprehensive checklists and validation
- **Release Planning & Coordination** with stakeholder communication templates
- **Rollback Planning** with automated recovery procedures
- **Hotfix Management** for emergency releases
- **Feature Flag Integration** for progressive rollouts

## Key Components

### Scripts

1. **changelog_generator.py** - Parses git logs and generates structured changelogs → verify: output exists + parses without error
2. **version_bumper.py** - Determines correct version bumps from conventional commits → verify: git status clean
3. **release_planner.py** - Assesses release readiness and generates coordination plans → verify: file content matches expected shape

### Documentation

- Comprehensive release management methodology
- Conventional commits specification and examples
- Release workflow comparisons (Git Flow, Trunk-based, GitHub Flow)
- Hotfix procedures and emergency response protocols

## Release Management Methodology

### Semantic Versioning (SemVer)

Semantic Versioning follows the MAJOR.MINOR.PATCH format where:

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backwards compatible manner  
- **PATCH** version when you make backwards compatible bug fixes

#### Pre-release Versions

Pre-release versions are denoted by appending a hyphen and identifiers:
- `1.0.0-alpha.1` - Alpha releases for early testing
- `1.0.0-beta.2` - Beta releases for wider testing
- `1.0.0-rc.1` - Release candidates for final validation

#### Version Precedence

Version precedence is determined by comparing each identifier:
1. `1.0.0-alpha` < `1.0.0-alpha.1` < `1.0.0-alpha.beta` < `1.0.0-beta` → verify: step output matches expected outcome
2. `1.0.0-beta` < `1.0.0-beta.2` < `1.0.0-beta.11` < `1.0.0-rc.1` → verify: step output matches expected outcome
3. `1.0.0-rc.1` < `1.0.0` → verify: step output matches expected outcome

### Conventional Commits

Conventional Commits provide a structured format for commit messages that enables automated tooling:

#### Format
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

#### Types
- **feat**: A new feature (correlates with MINOR version bump)
- **fix**: A bug fix (correlates with PATCH version bump)
- **docs**: Documentation only changes
- **style**: Changes that do not affect the meaning of the code
- **refactor**: A code change that neither fixes a bug nor adds a feature
- **perf**: A code change that improves performance
- **test**: Adding missing tests or correcting existing tests
- **chore**: Changes to the build process or auxiliary tools
- **ci**: Changes to CI configuration files and scripts
- **build**: Changes that affect the build system or external dependencies
- **breaking**: Introduces a breaking change (correlates with MAJOR version bump)

#### Examples
```
feat(user-auth): add OAuth2 integration

fix(api): resolve race condition in user creation

docs(readme): update installation instructions

feat!: remove deprecated payment API
BREAKING CHANGE: The legacy payment API has been removed
```

### Automated Changelog Generation

Changelogs are automatically generated from conventional commits, organized by:

#### Structure
```markdown
# Changelog

## [Unreleased]
### Added
### Changed  
### Deprecated
### Removed
### Fixed
### Security

## [1.2.0] - 2024-01-15
### Added
- OAuth2 authentication support (#123)
- User preference dashboard (#145)

### Fixed
- Race condition in user creation (#134)
- Memory leak in image processing (#156)

### Breaking Changes
- Removed legacy payment API
```

#### Grouping Rules
- **Added** for new features (feat)
- **Fixed** for bug fixes (fix)
- **Changed** for changes in existing functionality
- **Deprecated** for soon-to-be removed features
- **Removed** for now removed features
- **Security** for vulnerability fixes

#### Metadata Extraction
- Link to pull requests and issues: `(#123)`
- Breaking changes highlighted prominently
- Scope-based grouping: `auth:`, `api:`, `ui:`
- Co-authored-by for contributor recognition

### Version Bump Strategies

Version bumps are determined by analyzing commits since the last release:

#### Automatic Detection Rules
1. **MAJOR**: Any commit with `BREAKING CHANGE` or `!` after type → verify: git status clean
2. **MINOR**: Any `feat` type commits without breaking changes → verify: git status clean
3. **PATCH**: `fix`, `perf`, `security` type commits → verify: git status clean
4. **NO BUMP**: `docs`, `style`, `test`, `chore`, `ci`, `build` only → verify: all checks pass

#### Pre-release Handling
```python
# Alpha: 1.0.0-alpha.1 → 1.0.0-alpha.2
# Beta: 1.0.0-alpha.5 → 1.0.0-beta.1  
# RC: 1.0.0-beta.3 → 1.0.0-rc.1
# Release: 1.0.0-rc.2 → 1.0.0
```

#### Multi-package Considerations
For monorepos with multiple packages:
- Analyze commits affecting each package independently
- Support scoped version bumps: `@scope/package@1.2.3`
- Generate coordinated release plans across packages

### Release Branch Workflows

#### Git Flow
```
main (production) ← release/1.2.0 ← develop ← feature/login
                                           ← hotfix/critical-fix
```

**Advantages:**
- Clear separation of concerns
- Stable main branch
- Parallel feature development
- Structured release process

**Process:**
1. Create release branch from develop: `git checkout -b release/1.2.0 develop` → verify: output exists + parses without error
2. Finalize release (version bump, changelog) → verify: step output matches expected outcome
3. Merge to main and develop → verify: step output matches expected outcome
4. Tag release: `git tag v1.2.0` → verify: step output matches expected outcome
5. Deploy from main → verify: step output matches expected outcome

#### Trunk-based Development
```
main ← feature/login (short-lived)
    ← feature/payment (short-lived)  
    ← hotfix/critical-fix
```

**Advantages:**
- Simplified workflow
- Faster integration
- Reduced merge conflicts
- Continuous integration friendly

**Process:**
1. Short-lived feature branches (1-3 days) → verify: step output matches expected outcome
2. Frequent commits to main → verify: git status clean
3. Feature flags for incomplete features → verify: step output matches expected outcome
4. Automated testing gates → verify: all checks pass
5. Deploy from main with feature toggles → verify: step output matches expected outcome

#### GitHub Flow
```
main ← feature/login
    ← hotfix/critical-fix
```

**Advantages:**
- Simple and lightweight
- Fast deployment cycle
- Good for web applications
- Minimal overhead

**Process:**
1. Create feature branch from main → verify: output exists + parses without error
2. Regular commits and pushes → verify: git status clean
3. Open pull request when ready → verify: file content matches expected shape
4. Deploy from feature branch for testing → verify: all checks pass
5. Merge to main and deploy → verify: step output matches expected outcome

### Feature Flag Integration

Feature flags enable safe, progressive rollouts:

#### Types of Feature Flags
- **Release flags**: Control feature visibility in production
- **Experiment flags**: A/B testing and gradual rollouts
- **Operational flags**: Circuit breakers and performance toggles
- **Permission flags**: Role-based feature access

#### Implementation Strategy
```python
# Progressive rollout example
if feature_flag("new_payment_flow", user_id):
    return new_payment_processor.process(payment)
else:
    return legacy_payment_processor.process(payment)
```

#### Release Coordination
1. Deploy code with feature behind flag (disabled) → verify: step output matches expected outcome
2. Gradually enable for percentage of users → verify: step output matches expected outcome
3. Monitor metrics and error rates → verify: step output matches expected outcome
4. Full rollout or quick rollback based on data → verify: step output matches expected outcome
5. Remove flag in subsequent release → verify: step output matches expected outcome

### Release Readiness Checklists

#### Pre-Release Validation
- [ ] All planned features implemented and tested
- [ ] Breaking changes documented with migration guide
- [ ] API documentation updated
- [ ] Database migrations tested
- [ ] Security review completed for sensitive changes
- [ ] Performance testing passed thresholds
- [ ] Internationalization strings updated
- [ ] Third-party integrations validated

#### Quality Gates
- [ ] Unit test coverage ≥ 85%
- [ ] Integration tests passing
- [ ] End-to-end tests passing
- [ ] Static analysis clean
- [ ] Security scan passed
- [ ] Dependency audit clean
- [ ] Load testing completed

#### Documentation Requirements
- [ ] CHANGELOG.md updated
- [ ] README.md reflects new features
- [ ] API documentation generated
- [ ] Migration guide written for breaking changes
- [ ] Deployment notes prepared
- [ ] Rollback procedure documented

#### Stakeholder Approvals
- [ ] Product Manager sign-off
- [ ] Engineering Lead approval
- [ ] QA validation complete
- [ ] Security team clearance
- [ ] Legal review (if applicable)
- [ ] Compliance check (if regulated)

### Deployment Coordination

#### Communication Plan
**Internal Stakeholders:**
- Engineering team: Technical changes and rollback procedures
- Product team: Feature descriptions and user impact
- Support team: Known issues and troubleshooting guides
- Sales team: Customer-facing changes and talking points

**External Communication:**
- Release notes for users
- API changelog for developers
- Migration guide for breaking changes
- Downtime notifications if applicable

#### Deployment Sequence
1. **Pre-deployment** (T-24h): Final validation, freeze code → verify: step output matches expected outcome
2. **Database migrations** (T-2h): Run and validate schema changes   → verify: command exit code 0
3. **Blue-green deployment** (T-0): Switch traffic gradually → verify: step output matches expected outcome
4. **Post-deployment** (T+1h): Monitor metrics and logs → verify: step output matches expected outcome
5. **Rollback window** (T+4h): Decision point for rollback → verify: step output matches expected outcome

#### Monitoring & Validation
- Application health checks
- Error rate monitoring
- Performance metrics tracking
- User experience monitoring
- Business metrics validation
- Third-party service integration health

### Hotfix Procedures

Hotfixes address critical production issues requiring immediate deployment:

#### Severity Classification
**P0 - Critical**: Complete system outage, data loss, security breach
- **SLA**: Fix within 2 hours
- **Process**: Emergency deployment, all hands on deck
- **Approval**: Engineering Lead + On-call Manager

**P1 - High**: Major feature broken, significant user impact
- **SLA**: Fix within 24 hours  
- **Process**: Expedited review and deployment
- **Approval**: Engineering Lead + Product Manager

**P2 - Medium**: Minor feature issues, limited user impact
- **SLA**: Fix in next release cycle
- **Process**: Normal review process
- **Approval**: Standard PR review

#### Emergency Response Process
1. **Incident declaration**: Page on-call team → verify: step output matches expected outcome
2. **Assessment**: Determine severity and impact → verify: step output matches expected outcome
3. **Hotfix branch**: Create from last stable release → verify: output exists + parses without error
4. **Minimal fix**: Address root cause only → verify: diff matches intended change
5. **Expedited testing**: Automated tests + manual validation → verify: all checks pass
6. **Emergency deployment**: Deploy to production → verify: step output matches expected outcome
7. **Post-incident**: Root cause analysis and prevention → verify: step output matches expected outcome

### Rollback Planning

Every release must have a tested rollback plan:

#### Rollback Triggers
- **Error rate spike**: >2x baseline within 30 minutes
- **Performance degradation**: >50% latency increase
- **Feature failures**: Core functionality broken
- **Security incident**: Vulnerability exploited
- **Data corruption**: Database integrity compromised

#### Rollback Types
**Code Rollback:**
- Revert to previous Docker image
- Database-compatible code changes only
- Feature flag disable preferred over code rollback

**Database Rollback:**
- Only for non-destructive migrations
- Data backup required before migration
- Forward-only migrations preferred (add columns, not drop)

**Infrastructure Rollback:**
- Blue-green deployment switch
- Load balancer configuration revert
- DNS changes (longer propagation time)

#### Automated Rollback
```python
# Example rollback automation
def monitor_deployment():
    if error_rate() > THRESHOLD:
        alert_oncall("Error rate spike detected")
        if auto_rollback_enabled():
            execute_rollback()
```

### Release Metrics & Analytics

#### Key Performance Indicators
- **Lead Time**: From commit to production
- **Deployment Frequency**: Releases per week/month
- **Mean Time to Recovery**: From incident to resolution
- **Change Failure Rate**: Percentage of releases causing incidents

#### Quality Metrics
- **Rollback Rate**: Percentage of releases rolled back
- **Hotfix Rate**: Hotfixes per regular release
- **Bug Escape Rate**: Production bugs per release
- **Time to Detection**: How quickly issues are identified

#### Process Metrics
- **Review Time**: Time spent in code review
- **Testing Time**: Automated + manual testing duration
- **Approval Cycle**: Time from PR to merge
- **Release Preparation**: Time spent on release activities

### Tool Integration

#### Version Control Systems
- **Git**: Primary VCS with conventional commit parsing
- **GitHub/GitLab**: Pull request automation and CI/CD
- **Bitbucket**: Pipeline integration and deployment gates

#### CI/CD Platforms
- **Jenkins**: Pipeline orchestration and deployment automation
- **GitHub Actions**: Workflow automation and release publishing
- **GitLab CI**: Integrated pipelines with environment management
- **CircleCI**: Container-based builds and deployments

#### Monitoring & Alerting
- **DataDog**: Application performance monitoring
- **New Relic**: Error tracking and performance insights
- **Sentry**: Error aggregation and release tracking
- **PagerDuty**: Incident response and escalation

#### Communication Platforms
- **Slack**: Release notifications and coordination
- **Microsoft Teams**: Stakeholder communication
- **Email**: External customer notifications
- **Status Pages**: Public incident communication

## Best Practices

### Release Planning
1. **Regular cadence**: Establish predictable release schedule → verify: step output matches expected outcome
2. **Feature freeze**: Lock changes 48h before release → verify: step output matches expected outcome
3. **Risk assessment**: Evaluate changes for potential impact → verify: step output matches expected outcome
4. **Stakeholder alignment**: Ensure all teams are prepared → verify: step output matches expected outcome

### Quality Assurance
1. **Automated testing**: Comprehensive test coverage → verify: all checks pass
2. **Staging environment**: Production-like testing environment → verify: all checks pass
3. **Canary releases**: Gradual rollout to subset of users → verify: step output matches expected outcome
4. **Monitoring**: Proactive issue detection → verify: step output matches expected outcome

### Communication
1. **Clear timelines**: Communicate schedules early → verify: step output matches expected outcome
2. **Regular updates**: Status reports during release process → verify: step output matches expected outcome
3. **Issue transparency**: Honest communication about problems → verify: step output matches expected outcome
4. **Post-mortems**: Learn from incidents and improve → verify: step output matches expected outcome

### Automation
1. **Reduce manual steps**: Automate repetitive tasks → verify: user confirms
2. **Consistent process**: Same steps every time → verify: step output matches expected outcome
3. **Audit trails**: Log all release activities → verify: findings count > 0 OR clean signal returned
4. **Self-service**: Enable teams to deploy safely → verify: step output matches expected outcome

## Common Anti-patterns

### Process Anti-patterns
- **Manual deployments**: Error-prone and inconsistent
- **Last-minute changes**: Risk introduction without proper testing
- **Skipping testing**: Deploying without validation
- **Poor communication**: Stakeholders unaware of changes

### Technical Anti-patterns
- **Monolithic releases**: Large, infrequent releases with high risk
- **Coupled deployments**: Services that must be deployed together
- **No rollback plan**: Unable to quickly recover from issues
- **Environment drift**: Production differs from staging

### Cultural Anti-patterns
- **Blame culture**: Fear of making changes or reporting issues
- **Hero culture**: Relying on individuals instead of process
- **Perfectionism**: Delaying releases for minor improvements
- **Risk aversion**: Avoiding necessary changes due to fear

## Getting Started

1. **Assessment**: Evaluate current release process and pain points → verify: step output matches expected outcome
2. **Tool setup**: Configure scripts for your repository → verify: step output matches expected outcome
3. **Process definition**: Choose appropriate workflow for your team → verify: step output matches expected outcome
4. **Automation**: Implement CI/CD pipelines and quality gates → verify: dependency resolves + import works
5. **Training**: Educate team on new processes and tools → verify: step output matches expected outcome
6. **Monitoring**: Set up metrics and alerting for releases → verify: step output matches expected outcome
7. **Iteration**: Continuously improve based on feedback and metrics → verify: step output matches expected outcome

The Release Manager skill transforms chaotic deployments into predictable, reliable releases that build confidence across your entire organization.

## When NOT to use

- Task is unrelated to release manager — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Release Manager needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for release manager
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving release manager
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
