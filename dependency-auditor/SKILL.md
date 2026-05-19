---
name: "dependency-auditor"
description: 'Comprehensive dependency vulnerability scanning and license compliance auditing across npm, pip, cargo, go.mod, m. Triggers: ''use dependency-auditor'', ''dependency auditor'', ''dependency-auditor task.'
allowed-tools: Bash, Glob, Grep, Read
---

## Quick Start

```bash
# Scan project for vulnerabilities and licenses
python scripts/dep_scanner.py /path/to/project

# Check license compliance
python scripts/license_checker.py /path/to/project --policy strict

# Plan dependency upgrades
python scripts/upgrade_planner.py deps.json --risk-threshold medium
```

For detailed usage instructions, see [README.md](README.md).

---

*This skill provides comprehensive dependency management capabilities essential for maintaining secure, compliant, and efficient software projects. Regular use helps teams stay ahead of security threats, maintain legal compliance, and optimize their dependency ecosystems.*

## When NOT to use

- Task is unrelated to dependency auditor — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Dependency Auditor needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for dependency auditor
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving dependency auditor
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
