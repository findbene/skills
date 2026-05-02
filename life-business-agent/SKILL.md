---
name: life-business-agent
description: "Personal life and business operating system for calendar management, goals, personal projects, and life admin automation outside of coding work. Use this skill any time personal life tasks need to be managed, business admin tasks need to be automated, personal goals need to be tracked, or life planning assistance is needed. Trigger immediately on: \"life admin\", \"personal task\", \"personal goal\", \"life planning\", \"business admin\", \"manage my life\", \"life agent\", \"personal project\", \"personal planning\", \"track goals\", \"life tasks\", \"business task\", \"life management\", \"personal calendar\". If someone asks for help with personal or life tasks outside of code this skill MUST trigger."
---

# Life & Business Agent System

Claude Code is not just a coding tool — it's a general-purpose agent for daily life and business organization.

## Core Architecture

Think like a **project manager** who delegates to agents:
1. **Centralized task system** — Track all tasks in one place
2. **Centralized workspace** — All context documents linked to tasks
3. **Calendar integration** — Shared scheduling
4. **Tool connections** — Calendar, Notion, task manager, email, etc.

## Scheduled Triggers & Workflows

### Morning Review (~7 AM daily)
1. Review tasks not completed yesterday
2. Analyze today's calendar events
3. Check email inbox for important messages
4. Review active project progress
5. Suggest tasks agent can complete autonomously
6. Create full schedule breakdown for the day
7. Text summary to user via Twilio SMS

### Evening Review
1. Analyze what was completed vs task list
2. Suggest tasks for autonomous overnight execution
3. Queue research and coding tasks for background execution

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
