---
name: zapier-tools
description: "Zapier automation and AI Actions trigger via MCP for connecting apps and invoking Zapier workflows from Claude Code. Use this skill any time Zapier workflows need to be triggered, Zapier AI Actions need to be called, or app-to-app automations via Zapier need to be invoked. Trigger immediately on: \"Zapier\", \"Zap\", \"Zapier workflow\", \"Zapier trigger\", \"Zapier action\", \"Zapier MCP\", \"trigger Zapier\", \"Zapier AI Action\", \"connect apps with Zapier\", \"Zapier automation\", \"send to Zapier\", \"Zapier integration\". If someone says \"trigger this Zapier workflow\" or \"connect these apps with Zapier\" this skill MUST trigger."
---

# Zapier Tools

Trigger Zapier AI Actions to connect Claude Code with 7,000+ apps.

## How It Works

Zapier MCP exposes your pre-configured **AI Actions** as callable tools. Each action you enable in your Zapier AI Actions dashboard becomes a tool Claude can invoke. The available tools are dynamic — they depend on what you've set up at https://actions.zapier.com.

**Connection:** Remote HTTP MCP at `https://mcp.zapier.com/api/v1/connect` (OAuth-authenticated).

## Setup

1. Go to https://actions.zapier.com
2. Click "Add a new action"
3. Choose the app and action (e.g., "Slack: Send Channel Message")
4. Configure defaults and confirm
5. The action becomes available as an MCP tool in Claude Code

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
1. Create a Jira ticket for the bug
2. Send a Slack message to the team with the ticket link
3. Add a row to the tracking spreadsheet

## Tool Naming Convention

Zapier tools follow the pattern: `zapier_[action_name]`

The exact tool names depend on your configured actions. Run a tool listing or check https://actions.zapier.com to see your current actions.

## Troubleshooting

### Auth/Connection Failures
The Zapier MCP uses OAuth via `https://mcp.zapier.com/api/v1/connect`. If connections fail:
1. Re-authenticate: Claude Code will prompt for OAuth when the token expires
2. Check https://actions.zapier.com to verify your actions are still active
3. Ensure your Zapier plan supports AI Actions

### Action Not Available
If an expected tool isn't showing up:
1. Verify the action is enabled at https://actions.zapier.com
2. Check that the connected app account is still authorized in Zapier
3. Try disabling and re-enabling the action

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
