---
name: gcal-tools
description: "Google Calendar management via MCP for creating events, finding free time, scheduling meetings, and managing calendar entries. Use this skill any time calendar events need to be created, meetings need to be scheduled, free time slots need to be found, or calendar management is required. Trigger immediately on: \"Google Calendar\", \"schedule meeting\", \"create event\", \"add to calendar\", \"find free time\", \"calendar invite\", \"when am I free\", \"book a time\", \"calendar\", \"schedule a call\", \"meeting slot\", \"reschedule\", \"check availability\", \"calendar event\". If someone says \"put this on my calendar\" or \"find a time for this meeting\" this skill MUST trigger."
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
1. `gcal_list_events` to find the event ID
2. `gcal_update_event(event_id="...", start="new time", end="new time")`

## Tips

- All times should include timezone offset (e.g., `-05:00` for EST)
- `gcal_find_my_free_time` respects working hours if set in Google Calendar
- Use `gcal_list_calendars` to find the right calendar ID if you have multiple calendars
- `gcal_respond_to_event` only works for events you've been invited to
