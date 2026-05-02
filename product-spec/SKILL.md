---
name: product-spec
description: "Comprehensive product specification and PRD creation for feature requirements, user stories, acceptance criteria, and product documentation. Use this skill any time a PRD needs to be written, product specifications need to be documented, feature requirements need to be formalized, or product decisions need to be structured. Trigger immediately on: \"PRD\", \"product spec\", \"product requirements\", \"requirements document\", \"feature spec\", \"user story\", \"acceptance criteria\", \"product documentation\", \"product brief\", \"write a spec\", \"document requirements\", \"product definition\", \"what should this build do\". If someone says \"write a PRD for this\" or \"document the requirements\" this skill MUST trigger."
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
