---
name: zapier-tools
description: 'Zapier automation and AI Actions trigger via MCP for connecting apps and invoking Zapier workflows from Claude Code. Triggers: "use zapier-tools", "zapier tools", "zapier task".'
allowed-tools: Glob, Grep, Read
---

# Zapier Tools

Trigger Zapier AI Actions to connect Claude Code with 7,000+ apps.

## How It Works

Zapier MCP exposes your pre-configured **AI Actions** as callable tools. Each action you enable in your Zapier AI Actions dashboard becomes a tool Claude can invoke. The available tools are dynamic — they depend on what you've set up at https://actions.zapier.com.

**Connection:** Remote HTTP MCP at `https://mcp.zapier.com/api/v1/connect` (OAuth-authenticated).

## Setup

1. Go to https://actions.zapier.com → verify: step output matches expected outcome
2. Click "Add a new action" → verify: dependency resolves + import works
3. Choose the app and action (e.g., "Slack: Send Channel Message") → verify: step output matches expected outcome
4. Configure defaults and confirm → verify: step output matches expected outcome
5. The action becomes available as an MCP tool in Claude Code → verify: step output matches expected outcome

## Common Actions to Enable

| Action | What It Does | Use Case |
|--------|-------------|----------|
| Slack: Send Channel Message | Post to a Slack channel | Status updates, notifications |
| Slack: Send Direct Message | DM someone on Slack | Alerts, direct communication |
| Notion: Create Page | Create a page in Notion | Documentation, meeting notes |
| Notion: Append Block | Add content to existing page | Logging, appending updates |
| Trello: Create Card | Add a card to a board | Task tracking |
| Asana: Create Task | Create an Asana task | Project management |
| Google Sheets: Create Row | Add a row to a spreadsheet | Data logging, tracking |
| Google Sheets: Update Row | Update an existing row | Status updates |
| HubSpot: Create Contact | Add a CRM contact | Lead capture |
| Todoist: Create Task | Add a todo item | Personal task management |
| Airtable: Create Record | Add a record to a base | Data collection |
| Discord: Send Message | Post to a Discord channel | Community notifications |
| Jira: Create Issue | Create a Jira ticket | Bug tracking, feature requests |
| Linear: Create Issue | Create a Linear issue | Engineering task tracking |
| Twilio: Send SMS | Send a text message | Urgent alerts |
| Microsoft Teams: Send Message | Post to a Teams channel | Enterprise communication |

## Typical Workflows

### Send a Status Update to Slack
```
zapier_send_slack_channel_message(
  channel="#dev-updates",
  message="Deployed v2.1 — RSVP form now supports dietary restrictions"
)
```

### Log Work to a Spreadsheet
```
zapier_create_google_sheets_row(
  spreadsheet="Project Tracker",
  worksheet="Tasks",
  data={"Task": "Fix auth flow", "Status": "Complete", "Date": "2026-03-06"}
)
```

### Create a Task from a Bug
```
zapier_create_linear_issue(
  title="Login button unresponsive on mobile Safari",
  description="Steps to reproduce: ...",
  priority="High"
)
```

### Chain Multiple Actions
1. Create a Jira ticket for the bug → verify: output file exists + no syntax error
2. Send a Slack message to the team with the ticket link → verify: step output matches expected outcome
3. Add a row to the tracking spreadsheet → verify: file readable + content matches expected shape

## Tool Naming Convention

Zapier tools follow the pattern: `zapier_[action_name]`

The exact tool names depend on your configured actions. Run a tool listing or check https://actions.zapier.com to see your current actions.

## Troubleshooting

### Auth/Connection Failures
The Zapier MCP uses OAuth via `https://mcp.zapier.com/api/v1/connect`. If connections fail:
1. Re-authenticate: Claude Code will prompt for OAuth when the token expires → verify: step output matches expected outcome
2. Check https://actions.zapier.com to verify your actions are still active
3. Ensure your Zapier plan supports AI Actions → verify: step output matches expected outcome

### Action Not Available
If an expected tool isn't showing up:
1. Verify the action is enabled at https://actions.zapier.com
2. Check that the connected app account is still authorized in Zapier → verify: all checks pass
3. Try disabling and re-enabling the action → verify: step output matches expected outcome

### Rate Limits
Zapier AI Actions have usage limits based on your plan. For high-volume operations, consider using the direct MCP for that service (e.g., gmail-tools for bulk email) instead of routing through Zapier.

## When to Use Zapier vs Direct MCPs

| Scenario | Use |
|----------|-----|
| Gmail email operations | `gmail-tools` (direct, faster) |
| Google Calendar events | `gcal-tools` (direct, faster) |
| GitHub PRs and issues | `github-tools` (direct, faster) |
| Supabase database ops | `supabase-tools` (direct, faster) |
| Slack messages | `zapier-tools` (no direct Slack MCP) |
| Notion pages | `zapier-tools` (no direct Notion MCP) |
| Cross-app workflows | `zapier-tools` (orchestrates multiple services) |
| Any app without a dedicated MCP | `zapier-tools` (7,000+ app coverage) |

Prefer direct MCPs when available — they're faster and more reliable. Use Zapier for apps without dedicated MCP integrations or for multi-app orchestration.

## When NOT to use

- Task is unrelated to zapier tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Zapier Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for zapier tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving zapier tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
