# Escalation Guide

## When to Escalate

### Immediate Tier 3 (No AI handling)
- Contains: "legal", "lawyer", "lawsuit", "police"
- Contains: "data breach", "hacked", "unauthorized access"
- Enterprise client churn risk (contract > $10K/year)

### Tier 2 (Human agent needed)
- Customer says "speak to human", "manager", "supervisor"
- AI confidence < 0.75
- Customer says "that didn't help" or "that's wrong" after AI response
- 3+ turns without resolution
- Billing dispute > $100
- Account deletion / data privacy request (GDPR/CCPA)
- Very negative sentiment + repeat contact

### Tier 1 AI handles
- Standard FAQ queries (billing questions, how-to, feature questions)
- Low-complexity technical support with knowledge base match
- Status inquiries

## Decision Tree

```
Query received
  ├── "legal", "lawsuit", "hacked" → IMMEDIATE Tier 3
  ├── "speak to human", "manager" → Tier 2
  ├── Sentiment = very negative + repeat contact → Tier 2
  ├── AI confidence < 0.75 → Tier 2
  └── Standard query → Tier 1 (AI handles)
```

## SLA Requirements

| Priority | First Response | Resolution |
|---|---|---|
| Critical | < 1 hour | < 4 hours |
| High | < 4 hours | < 24 hours |
| Medium | < 8 hours | < 48 hours |
| Low | < 24 hours | < 72 hours |

## SLA Breach Detection

```python
def check_sla_breaches(supabase_client):
    """Run every 15 minutes via n8n schedule trigger."""
    from datetime import datetime

    tickets = supabase_client.table("support_tickets")\
        .select("*")\
        .eq("status", "open")\
        .execute()

    alerts = []
    for ticket in tickets.data:
        created = datetime.fromisoformat(ticket["created_at"])
        hours_elapsed = (datetime.utcnow() - created).total_seconds() / 3600
        sla = SLA_TARGETS[ticket["priority"]]

        if not ticket.get("first_response_at"):
            warn_threshold = sla["first_response"] * 0.8
            if hours_elapsed > warn_threshold:
                alerts.append({
                    "ticket_id": ticket["id"],
                    "type": "first_response_approaching",
                    "priority": ticket["priority"]
                })

    return alerts
```

## Handoff Message Templates

### To Human Agent
```
Subject: [Ticket #{ticket_id}] Needs human assistance

Customer: {customer_name}
Issue: {summary}
Escalation reason: {reason}
Priority: {priority} | SLA deadline: {deadline}
Conversation: {link}
```

### Customer Acknowledgment
```
Hi {first_name},

I've connected you with our team — they'll be in touch within {sla_hours} hours.
Your reference: #{ticket_id}

[Support Team]
```
