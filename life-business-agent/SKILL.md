---
name: life-business-agent
description: 'Personal life and business operating system for calendar management, goals, personal projects, and life admin automation outsid. Triggers: "use life-business-agent", "life business agent", "life task.'
allowed-tools: Glob, Grep, Read
---

# Life & Business Agent System

Claude Code is not just a coding tool — it's a general-purpose agent for daily life and business organization.

## Core Architecture

Think like a **project manager** who delegates to agents:
1. **Centralized task system** — Track all tasks in one place → verify: user confirms
2. **Centralized workspace** — All context documents linked to tasks → verify: user confirms
3. **Calendar integration** — Shared scheduling → verify: step output matches expected outcome
4. **Tool connections** — Calendar, Notion, task manager, email, etc. → verify: user confirms

## Scheduled Triggers & Workflows

### Morning Review (~7 AM daily)
1. Review tasks not completed yesterday → verify: step output matches expected outcome
2. Analyze today's calendar events → verify: step output matches expected outcome
3. Check email inbox for important messages → verify: all tests pass
4. Review active project progress → verify: step output matches expected outcome
5. Suggest tasks agent can complete autonomously → verify: step output matches expected outcome
6. Create full schedule breakdown for the day → verify: output file exists + no syntax error
7. Text summary to user via Twilio SMS → verify: step output matches expected outcome

### Evening Review
1. Analyze what was completed vs task list → verify: step output matches expected outcome
2. Suggest tasks for autonomous overnight execution → verify: step output matches expected outcome
3. Queue research and coding tasks for background execution → verify: step output matches expected outcome

### Overnight Autonomous Work
Agent works 6+ hours while you sleep: complete coding tasks, assemble research, draft content, build dashboards, analyze metrics, send morning summary.

## Task Ingestion System

### Apple Reminders → Task Files
Say "Hey Siri, add a reminder" → script scrapes every ~15 minutes → converts to task markdown in Obsidian vault → agent processes.

### SMS Interface (Twilio)
Dedicated phone number for your agent. Text tasks and questions, get responses and notifications.

### Telegram Bot
Full Claude Code accessible from phone — continue sessions, get notifications, conduct reviews on the go.

## Business Management

- Integrate scattered systems: Google Drive, Notion, Jira, Slack, CRM, ads
- Natural language interface to every moving part
- Content automation: scrape trends, draft scripts, create posts, analyze engagement

## Key Integrations

| Integration | Method | Purpose |
|------------|--------|---------|
| Calendar | MCP server | Scheduling, reminders |
| Email/Gmail | MCP server | Inbox analysis, drafting |
| SMS | Twilio scripts | Mobile access, notifications |
| Telegram | Bot API | Mobile Claude Code |
| Notion | MCP server | Notes, task management |
| Obsidian | Local files | Memory, knowledge base |
| n8n | MCP server + skills | Workflow automation |

## Proactive Agent (CloudBot/OpenClaw)

### Heartbeat Mechanism
- Wakes every ~30 minutes, reads `heartbeat.md` for standard procedure
- Tackles tasks autonomously
- Scheduled cron jobs for time-sensitive tasks

### Continual Learning
- Snippets from conversations → daily memory files
- Memory files vectorized for semantic search

## When NOT to use

- Codebase-focused engineering work — use the appropriate dev skill
- Pure CRM operations — use `crm-integration`
- Calendar booking for sales leads — use `customer-success-manager` or scheduling tool
- Anything with hard SLA or commercial obligation — keep humans in the loop
- One-off task that doesn't need persistent agent — just do it

## Red Flags

| Thought | Reality |
|---------|---------|
| "Let agent auto-send important emails" | Risk of irreversible mistakes; queue drafts, human approves |
| "Skip morning review, agent will catch up" | Without daily sync, agent state drifts and tasks rot |
| "One mega memory file works" | Daily memory files + vectorization scale; single file does not |
| "Trust the cron to run forever" | Monitor heartbeat; silent failure = invisible drift |

## Output Contract

Done when:
- Centralized task store wired (Obsidian / Notion / Apple Reminders)
- Calendar MCP connected
- Morning + evening review workflows scheduled
- Overnight autonomous workflow queue defined
- SMS (Twilio) and/or Telegram bot interface configured
- Heartbeat agent running with `heartbeat.md` procedure
- Daily memory files captured and indexed
- Manual-approval gate on irreversible actions (sending, booking, paying)

## Examples

### Example 1 — Onboarding a new user's life OS
- Input: "Set up my life agent"
- Action: Connect Calendar + Gmail + Notion + Twilio MCPs, draft morning + evening prompts, schedule cron, write `heartbeat.md`, seed task list with current week
- Output: Live agent running 7am+evening reviews, SMS interface tested, first memory file written

### Example 2 — Overnight async work
- Input: "While I sleep, research three competitors and draft a 1-page brief on each"
- Action: Queue 3 background research subagents with explicit scope, write findings to `briefs/`, send morning SMS summary with links
- Output: Three competitor briefs in vault by 7am with one-paragraph SMS summary
