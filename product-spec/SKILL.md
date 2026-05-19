---
name: product-spec
description: 'Comprehensive product specification and PRD creation for feature requirements, user stories, acceptance criteria, and product documentatio. Triggers: "use product-spec", "product spec", "product task.'
allowed-tools: Glob, Grep, Read
---

# Product Specification

Guide for creating effective product specifications and PRDs.

## Document Structure

### 1. Overview
- **Problem Statement**: What problem are we solving? Why now?
- **Target Users**: Who specifically will use this?
- **Success Metrics**: How will we measure success?

### 2. Background & Context
- Current state and pain points
- Previous attempts or related work
- Market/competitive context
- Technical constraints

### 3. Requirements

#### Functional Requirements
- Core features (must-have)
- Secondary features (nice-to-have)
- Out of scope (explicitly excluded)

#### Non-Functional Requirements
- Performance (latency, throughput)
- Scalability expectations
- Security requirements
- Accessibility standards

### 4. User Stories & Flows
```
As a [user type], I want to [action] so that [benefit].
```

Include primary user flows, edge cases, and error states.

### 5. Acceptance Criteria
- Specific, testable conditions
- Definition of done
- Quality gates

### 6. Technical Considerations
- Architecture implications
- API requirements
- Data model changes
- Integration points

### 7. Timeline & Milestones
- Phases and deliverables
- Dependencies
- Risk factors

## Best Practices

- Start with the "why" before the "what"
- Be specific about scope boundaries
- Include both happy path and edge cases
- Write testable acceptance criteria
- Document assumptions explicitly
- Get stakeholder alignment early

## When NOT to use

- Engineering implementation plan or technical design doc — use `senior-architect` or `plan` skill
- Marketing positioning / messaging — use `product-marketing-context` or `marketing-strategy-pmm`
- Roadmap (multi-feature, multi-quarter) — use `roadmap-communicator` or `senior-pm`
- Single-bug fix or trivial task — overkill; write a one-line ticket
- Discovery phase (still figuring out the problem) — use `product-discovery` first

## Red Flags

| Rationalization | Reality |
|---|---|
| "Skip acceptance criteria, engineering will figure it out" | Untestable criteria produces scope creep and missed expectations — write them as Given/When/Then |
| "Goals do not need metrics" | Without success metrics there is no way to evaluate the launch; force quantification |
| "Out of scope is obvious, no need to write it" | Explicit out-of-scope prevents 80% of scope-creep arguments |
| "Personas can be skipped, we know the user" | Even one paragraph forces specificity — "target user" without a persona drifts to "everyone" |

## Output Contract

Finished output must contain:
- Problem Statement, Target User, Success Metrics in section 1
- Functional Requirements (must-have, nice-to-have, explicitly out of scope)
- Non-functional Requirements (performance, security, accessibility, compliance)
- Acceptance Criteria as Given/When/Then or testable checklist
- Open questions list with named owners
- Dependencies (other teams, services, vendors)
- Launch plan (rollout, monitoring, rollback)

## Examples

**Example 1 — New feature PRD**
- Input: "Write a PRD for adding 2FA to our SaaS app"
- Action: Problem (account takeover risk, GDPR) → user (security-conscious admin) → metrics (% accounts with 2FA enabled, support tickets) → functional (TOTP + SMS, recovery codes, enforcement policy for admins) → out of scope (hardware keys, biometrics)
- Output: 6-section PRD with G/W/T acceptance criteria, 5 open questions assigned, rollout plan (opt-in → admin-mandatory → all)

**Example 2 — Bug-batch product brief**
- Input: "We have 12 checkout bugs; write a PRD for a checkout reliability sprint"
- Action: Group bugs by root cause → define success (checkout error rate <0.1%, conversion lift) → acceptance criteria per category → explicit out-of-scope (no UI redesign)
- Output: PRD with quantified targets, prioritized backlog mapped to PRD requirements, rollback plan documented
