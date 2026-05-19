---
name: sentry-tools
description: 'Sentry error monitoring and production issue management via MCP for investigating bugs, tracking errors, and analyzing incidents. Triggers: "use sentry-tools", "sentry tools", "sentry task".'
allowed-tools: Glob, Grep, Read
---

# Sentry Tools

Monitor, investigate, and manage production errors.

## Available Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `search_issues` | Search for Sentry issues by query | Finding specific errors |
| `get_issue_details` | Get full details of a specific issue | Deep-diving into an error |
| `search_events` | Search error events with filters | Analyzing error patterns |
| `search_issue_events` | Search events within a specific issue | Understanding issue frequency |
| `get_trace_details` | View distributed trace data | Tracing errors across services |
| `get_issue_tag_values` | Get tag breakdown for an issue | Understanding who/what is affected |
| `analyze_issue_with_seer` | AI-powered root cause analysis | Quick automated diagnosis |
| `update_issue` | Change issue status/assignment | Triaging and resolving |
| `create_project` | Create a new Sentry project | Setting up monitoring |
| `create_team` | Create a new team | Organizing access |
| `find_organizations` | List organizations | Finding your org |
| `find_projects` | List projects in an org | Navigating projects |
| `create_dsn` / `find_dsns` | Manage DSN keys | SDK configuration |

## Common Workflows

### Investigate a Production Error
1. `search_issues(query="error message or keyword")` to find the issue → verify: step output matches expected outcome
2. `get_issue_details(issue_id="...")` for full stack trace and metadata → verify: step output matches expected outcome
3. `get_issue_tag_values(issue_id="...", tag_key="browser")` to see affected users → verify: step output matches expected outcome
4. `analyze_issue_with_seer(issue_id="...")` for AI root cause analysis → verify: step output matches expected outcome

### Check Recent Errors
1. `find_projects` to get the project slug → verify: step output matches expected outcome
2. `search_events(project="slug", query="is:unresolved", sort="-timestamp")` for latest errors → verify: all tests pass
3. Triage by severity and frequency → verify: step output matches expected outcome

### Trace a Request Across Services
1. Get the trace ID from an error event → verify: step output matches expected outcome
2. `get_trace_details(trace_id="...")` to see the full distributed trace → verify: step output matches expected outcome
3. Identify which service introduced the error → verify: step output matches expected outcome

### Resolve and Track
1. `update_issue(issue_id="...", status="resolved")` after fixing → verify: diff matches intended change
2. Monitor for regressions via `search_issues` → verify: step output matches expected outcome

## Tips

- Use `is:unresolved` in queries to filter to active issues
- Sort by `-times_seen` to find the most impactful errors
- Tag values reveal patterns: check browser, OS, user, and URL tags
- Seer's AI analysis works best on issues with clear stack traces

## When NOT to use

- Task doesn't involve Sentry error tracking integration → use the matching domain skill instead
- Simple one-off operation that doesn't need this skill's structure
- Different toolchain required → check `find-skills` skill for alternatives
- User explicitly asks to skip skill discipline → respect the override

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll skip the verify step, output looks right" | Eyeballing without verification ships broken outputs |
| "Generic answer is good enough" | Sentry observability needs domain-specific decisions, not boilerplate |
| "I can hold all context in head" | Multi-step state loss creates regressions you won't catch |
| "Just one more shortcut" | Shortcuts compound — finish discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced and matches user's stated goal
- All verification steps in process passed
- Edge cases for Sentry error tracking integration addressed or explicitly noted
- Output is reproducible (no hidden state)
- Hand-off summary provided so user can validate without re-reading entire flow

## Examples

### Example 1 — golden path
- Input: standard request involving Sentry error tracking integration
- Action: follow the documented numbered process, apply verify clauses per step
- Output: deliverable that passes the Output Contract

### Example 2 — edge case
- Input: request with non-standard constraint or partial info
- Action: detect the gap, ask clarifying question OR document assumption, proceed with adapted process
- Output: deliverable + explicit note on assumption/limitation
