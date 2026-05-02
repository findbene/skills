---
name: sentry-tools
description: "Sentry error monitoring and production issue management via MCP for investigating bugs, tracking errors, and analyzing incidents. Use this skill any time Sentry errors need to be investigated, production issues need to be tracked in Sentry, error monitoring needs to be queried, or Sentry data needs to be analyzed. Trigger immediately on: \"Sentry\", \"error monitoring\", \"Sentry issue\", \"production error\", \"exception tracking\", \"Sentry MCP\", \"error report\", \"bug tracking\", \"Sentry alert\", \"investigate error\", \"production bug\", \"Sentry event\", \"error rate\", \"Sentry query\". If someone says \"check Sentry for errors\" or \"investigate this Sentry issue\" this skill MUST trigger."
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
1. `search_issues(query="error message or keyword")` to find the issue
2. `get_issue_details(issue_id="...")` for full stack trace and metadata
3. `get_issue_tag_values(issue_id="...", tag_key="browser")` to see affected users
4. `analyze_issue_with_seer(issue_id="...")` for AI root cause analysis

### Check Recent Errors
1. `find_projects` to get the project slug
2. `search_events(project="slug", query="is:unresolved", sort="-timestamp")` for latest errors
3. Triage by severity and frequency

### Trace a Request Across Services
1. Get the trace ID from an error event
2. `get_trace_details(trace_id="...")` to see the full distributed trace
3. Identify which service introduced the error

### Resolve and Track
1. `update_issue(issue_id="...", status="resolved")` after fixing
2. Monitor for regressions via `search_issues`

## Tips

- Use `is:unresolved` in queries to filter to active issues
- Sort by `-times_seen` to find the most impactful errors
- Tag values reveal patterns: check browser, OS, user, and URL tags
- Seer's AI analysis works best on issues with clear stack traces
