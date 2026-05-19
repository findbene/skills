---
name: gmail-tools
description: 'Gmail email management via MCP for reading, searching, drafting, and managing emails and threads. Trigger: ''Gmail'', ''email'', ''read my email'', ''search emails'', ''draft email.'
allowed-tools: Glob, Grep, Read
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
1. `gmail_search_messages(query="from:john subject:invoice")` to find it → verify: step output matches expected outcome
2. `gmail_read_message(message_id="...")` for full content → verify: file readable + content matches expected shape

### Draft a Reply
1. `gmail_read_thread(thread_id="...")` to read the full conversation → verify: file readable + content matches expected shape
2. `gmail_create_draft(to="...", subject="Re: ...", body="...")` to compose → verify: output file exists + no syntax error

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

## When NOT to use

- Task is unrelated to gmail tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Gmail Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for gmail tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving gmail tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
