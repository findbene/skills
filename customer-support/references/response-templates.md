# Support Response Templates

## Standard Templates

### Billing Query
```
Hi {first_name},

{answer_from_knowledge_base}

If you need to update payment details or have questions about a specific charge,
reply with the details and we'll take care of it right away.

Support Team
```

### Technical Issue (With Fix)
```
Hi {first_name},

Here's how to fix it:
1. {step_1}
2. {step_2}
3. {step_3}

This should resolve it immediately. Let me know if you're still seeing the issue.

{agent_name}
```

### Technical Issue (Under Investigation)
```
Hi {first_name},

Thanks for the report — our team is actively working on this.
Estimated resolution: {eta}

I'll send you an update as soon as it's fixed. No action needed on your end.
Reference: #{ticket_id}

{agent_name}
```

### Escalation Acknowledgment
```
Hi {first_name},

I've flagged this for {specialist_team} who handles {issue_type}.
They'll be in touch within {sla}.

Your ticket reference: #{ticket_id}

{agent_name}
```

---

## Tone Guidelines

### Always
- Use first name (never "Dear Customer")
- State what happens next
- Use "I" for personal accountability ("I'll look into this")
- One issue per reply — don't bundle

### Never
- "As per my previous email" — condescending
- "Unfortunately" as first word
- "No problem" — implies it was a problem
- "I'm sorry you feel that way" — dismissive
- Internal jargon

### Tone by Sentiment
| Sentiment | Tone |
|---|---|
| Neutral | Professional, direct |
| Frustrated | Empathetic first, then solve |
| Angry | Calm, zero defensiveness, focus on resolution |
| Happy | Warm and conversational |
| Confused | Patient, extra detail, numbered steps |

---

## Citadel AI Specific Scenarios

### Video quality concerns
```
We use a multi-tier quality pipeline — every output is scored before delivery.
If a specific video is below expectations, send it to us and we'll investigate immediately.
We want to make it right.
```

### Turnaround time
```
Standard delivery is [X days] from brief submission.
For 48h rush requests, we can accommodate — reply to confirm availability and pricing.
```

### Custom niche request
```
We support custom niches outside our standard categories.
Send us a brief description of your niche and target audience — we'll scope it within 24h.
```
