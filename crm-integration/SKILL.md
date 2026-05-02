---
name: crm-integration
description: "CRM integration specialist for Salesforce, HubSpot, and other CRMs covering data sync, pipeline management, and customer data operations. Use this skill any time CRM data needs to be accessed or updated, HubSpot or Salesforce integrations need to be built, or CRM automations need to be created. Trigger immediately on: \"CRM\", \"HubSpot\", \"Salesforce\", \"CRM integration\", \"pipeline management\", \"customer data\", \"contact\", \"deal stage\", \"CRM sync\", \"HubSpot API\", \"Salesforce API\", \"CRM automation\", \"lead management\", \"CRM webhook\". If someone says \"update HubSpot\" or \"sync data with the CRM\" this skill MUST trigger."
---

# CRM Integration & Customer Management

Implement CRM integrations, customer data management, interaction tracking, and relationship analytics.

## Customer Entity Model

```typescript
interface Customer {
  id: string;
  email: string;
  firstName: string;
  lastName: string;
  segment: 'lead' | 'prospect' | 'customer' | 'churned';
  tier: 'free' | 'basic' | 'premium' | 'enterprise';
  lifetimeValue: number;
  totalOrders: number;
  lastActivityDate: Date;
  communicationPreferences: { email: boolean; sms: boolean; push: boolean };
  tags: string[];
  customFields: Record<string, any>;
}
```

## CRM Integration Patterns

### Salesforce
Use `jsforce` for connection. Upsert contacts by email to avoid duplicates. Query activity history with SOQL.

### HubSpot
Use `@hubspot/api-client`. Create-or-update pattern: try create first, catch duplicate error, then update by email lookup.

### Webhook Sync
Handle inbound CRM events (contact.created, contact.updated, deal.closed) via webhook endpoints. Implement outbound sync with status tracking and error recording for retry logic.

## Customer Segmentation

### Automatic Segmentation
Define segment rules with conditions (field, operator, value) and evaluate customers against them. Common segments: high_value, at_risk, new_lead.

### RFM Analysis
Score customers on Recency (days since last purchase), Frequency (number of purchases), and Monetary (total spend) on a 1-5 scale. Map scores to segments: champions, loyal_customers, at_risk, hibernating, lost.

RFM is valuable because it uses actual purchase behavior rather than demographic assumptions — customers who bought recently, frequently, and spent more are genuinely more engaged.

## Best Practices

1. **Single source of truth** — Sync data bidirectionally
2. **Deduplication** — Merge duplicate contacts on import
3. **Data hygiene** — Regular cleanup and validation
4. **Privacy compliance** — Respect opt-outs and GDPR
5. **Activity tracking** — Log all touchpoints
6. **Integration testing** — Verify sync reliability
7. **Audit trail** — Track all data changes
