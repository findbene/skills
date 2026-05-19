---
name: gcal-tools
description: 'Google Calendar management via MCP for creating events, finding free time, scheduling meetings, and managing calendar entries. Triggers: "use gcal-tools", "gcal tools", "gcal task".'
allowed-tools: Glob, Grep, Read
---

# Google Calendar Tools

Manage events, find free time, and schedule meetings.

## Available Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `gcal_list_events` | List upcoming events | Checking your schedule |
| `gcal_get_event` | Get details of a specific event | Reading event info |
| `gcal_create_event` | Create a new event | Scheduling meetings or blocks |
| `gcal_update_event` | Modify an existing event | Rescheduling or editing details |
| `gcal_delete_event` | Remove an event | Canceling meetings |
| `gcal_find_my_free_time` | Find your available time slots | Planning your day |
| `gcal_find_meeting_times` | Find mutual free time with others | Coordinating group meetings |
| `gcal_respond_to_event` | Accept/decline/tentative an invite | RSVP to meetings |
| `gcal_list_calendars` | List all calendars | Seeing available calendars |

## Common Workflows

### Check Today's Schedule
```
gcal_list_events(time_min="2026-03-06T00:00:00Z", time_max="2026-03-06T23:59:59Z")
```

### Find Free Time This Week
```
gcal_find_my_free_time(
  time_min="2026-03-06T09:00:00Z",
  time_max="2026-03-10T17:00:00Z"
)
```
Returns available blocks between your events.

### Schedule a Meeting
```
gcal_create_event(
  summary="Team Standup",
  start="2026-03-07T10:00:00-05:00",
  end="2026-03-07T10:30:00-05:00",
  attendees=["alice@example.com", "bob@example.com"],
  description="Weekly sync"
)
```

### Find Time With Others
```
gcal_find_meeting_times(
  attendees=["alice@example.com", "bob@example.com"],
  duration_minutes=60,
  time_min="2026-03-07T09:00:00Z",
  time_max="2026-03-14T17:00:00Z"
)
```

### Reschedule an Event
1. `gcal_list_events` to find the event ID → verify: step output matches expected outcome
2. `gcal_update_event(event_id="...", start="new time", end="new time")` → verify: step output matches expected outcome

## Tips

- All times should include timezone offset (e.g., `-05:00` for EST)
- `gcal_find_my_free_time` respects working hours if set in Google Calendar
- Use `gcal_list_calendars` to find the right calendar ID if you have multiple calendars
- `gcal_respond_to_event` only works for events you've been invited to

## When NOT to use

- Task is unrelated to gcal tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Gcal Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for gcal tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving gcal tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
