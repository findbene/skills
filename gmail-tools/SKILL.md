---
name: gmail-tools
description: "Gmail email management via MCP for reading, searching, drafting, and managing emails and threads. Use this skill any time emails need to be read, email threads need to be searched, draft emails need to be created, or Gmail needs to be managed programmatically. Trigger immediately on: \"Gmail\", \"email\", \"read my email\", \"search emails\", \"draft email\", \"send email\", \"email thread\", \"Gmail API\", \"email inbox\", \"compose email\", \"email subject\", \"email search\", \"Gmail MCP\", \"check email\". If someone says \"read my emails\" or \"search for an email about X\" this skill MUST trigger."
---

# Gmail Tools

Read, search, and draft emails in Gmail.

## Available Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `gmail_search_messages` | Search emails by query | Finding specific emails |
| `gmail_read_message` | Read a specific email by ID | Reading full email content |
| `gmail_read_thread` | Read an entire email thread | Following a conversation |
| `gmail_create_draft` | Draft a new email | Composing replies or new messages |
| `gmail_list_drafts` | List existing drafts | Reviewing unsent emails |
| `gmail_list_labels` | List email labels/folders | Understanding inbox organization |
| `gmail_get_profile` | Get account profile info | Checking connected account |

## Common Workflows

### Find and Read an Email
1. `gmail_search_messages(query="from:john subject:invoice")` to find it
2. `gmail_read_message(message_id="...")` for full content

### Draft a Reply
1. `gmail_read_thread(thread_id="...")` to read the full conversation
2. `gmail_create_draft(to="...", subject="Re: ...", body="...")` to compose

### Search Patterns
| Query | What It Finds |
|-------|--------------|
| `from:name@email.com` | Emails from a specific sender |
| `subject:invoice` | Emails with "invoice" in subject |
| `after:2025/01/01 before:2025/03/01` | Date range |
| `has:attachment filename:pdf` | PDFs attached |
| `is:unread` | Unread emails |
| `label:important` | Labeled as important |
| `"exact phrase"` | Exact text match |

Combine queries: `from:boss@company.com after:2025/02/01 has:attachment`

## Tips

- `gmail_create_draft` creates a draft — it does not send. The user sends manually from Gmail.
- Use `gmail_read_thread` instead of reading individual messages to get full conversation context
- Gmail search syntax is the same as the Gmail web search bar
- Check `gmail_get_profile` first to confirm which account is connected
