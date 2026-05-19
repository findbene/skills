---
name: roadmap-communicator
description: 'Use when preparing roadmap narratives, release notes, changelogs, or stakeholder updates tailored for executives, engineer. Triggers: "use roadmap-communicator", "roadmap communicator", "roadmap task.'
allowed-tools: Bash, Glob, Grep, Read
---

# Roadmap Communicator

Create clear roadmap communication artifacts for internal and external stakeholders.

## When To Use

Use this skill for:
- Building roadmap presentations in different formats
- Writing stakeholder updates (board, engineering, customers)
- Producing release notes (user-facing and internal)
- Generating changelogs from git history
- Structuring feature announcements

## Roadmap Formats

1. Now / Next / Later → verify: step output matches expected outcome
- Best for uncertainty and strategic flexibility.
- Communicate direction without false precision.

2. Timeline roadmap → verify: step output matches expected outcome
- Best for fixed-date commitments and launch coordination.
- Requires active risk and dependency management.

3. Theme-based roadmap → verify: step output matches expected outcome
- Best for outcome-led planning and cross-team alignment.
- Groups initiatives by problem space or strategic objective.

See `references/roadmap-templates.md` for templates.

## Stakeholder Update Patterns

### Board / Executive
- Outcome and risk oriented
- Focus on progress against strategic goals
- Highlight trade-offs and required decisions

### Engineering
- Scope, dependencies, and sequencing clarity
- Status, blockers, and resourcing implications

### Customers
- Value narrative and timing window
- What is available now vs upcoming
- Clear expectation setting

See `references/communication-templates.md` for reusable templates.

## Release Notes Guidance

### User-Facing Release Notes
- Lead with user value, not internal implementation details.
- Group by workflows or user jobs.
- Include migration/behavior changes explicitly.

### Internal Release Notes
- Include technical details, operational impact, and known issues.
- Capture rollout plan, rollback criteria, and monitoring notes.

## Changelog Generation

Use:
```bash
python3 scripts/changelog_generator.py --from v1.0.0 --to HEAD
```

Features:
- Reads git log range
- Parses conventional commit prefixes
- Groups entries by type (`feat`, `fix`, `chore`, etc.)
- Outputs markdown or plain text

## Feature Announcement Framework

1. Problem context → verify: step output matches expected outcome
2. What changed → verify: step output matches expected outcome
3. Why it matters → verify: step output matches expected outcome
4. Who benefits most → verify: step output matches expected outcome
5. How to get started → verify: step output matches expected outcome
6. Call to action and feedback channel → verify: step output matches expected outcome

## Communication Quality Checklist

- [ ] Audience-specific framing is explicit.
- [ ] Outcomes and trade-offs are clear.
- [ ] Terminology is consistent across artifacts.
- [ ] Risks and dependencies are not hidden.
- [ ] Next actions and owners are specified.

## When NOT to use

- Task is unrelated to roadmap communicator — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Roadmap Communicator needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for roadmap communicator
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving roadmap communicator
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
