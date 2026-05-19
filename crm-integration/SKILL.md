---
name: crm-integration
description: 'CRM integration specialist for Salesforce, HubSpot, and other CRMs covering data sync, pipeline management, and customer data operations. Triggers: "use crm-integration", "crm integration", "crm task.'
allowed-tools: Glob, Grep, Read
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

1. **Single source of truth** — Sync data bidirectionally → verify: step output matches expected outcome
2. **Deduplication** — Merge duplicate contacts on import → verify: step output matches expected outcome
3. **Data hygiene** — Regular cleanup and validation → verify: step output matches expected outcome
4. **Privacy compliance** — Respect opt-outs and GDPR → verify: step output matches expected outcome
5. **Activity tracking** — Log all touchpoints → verify: step output matches expected outcome
6. **Integration testing** — Verify sync reliability
7. **Audit trail** — Track all data changes → verify: findings count > 0 OR clean signal returned

## When NOT to use

- Pure marketing-automation flows (email sequences) — use `email-sequence` or `marketing-ops`
- Customer-support ticket routing — use `customer-service-ai` or `support-ticket-triage`
- Sales-pipeline coaching/strategy — use `sales-coach` or `deal-strategist`
- Pure data warehouse / ETL into BI — use `data-engineer` or `snowflake-development`
- CRM admin tasks (user management, layouts) inside the CRM UI — out of scope

## Red Flags

| Thought | Reality |
|---------|---------|
| "Create then update without upsert" | Race conditions create duplicate contacts; always upsert by email |
| "Ignore inbound webhooks, we'll batch nightly" | Stale views in CRM = sales acting on wrong data; webhooks for state changes |
| "Skip retry on failed sync" | Lost data forever; status + retry queue is mandatory |
| "GDPR is the privacy team's problem" | Opt-out flags must be respected at sync layer or you ship a violation |

## Output Contract

Done when:
- Customer entity model defined with required + optional fields
- CRM provider chosen and SDK wired (`jsforce` for Salesforce, `@hubspot/api-client` for HubSpot)
- Bidirectional sync paths defined (inbound webhook + outbound API)
- Upsert-by-email pattern used to prevent dupes
- Retry queue + error log for failed syncs
- Segmentation rules / RFM scoring implemented if required
- GDPR/CCPA opt-out flags honored end-to-end

## Examples

### Example 1 — HubSpot two-way sync
- Input: "Sync our app's user signups + activity into HubSpot"
- Action: Outbound: on signup, upsert HubSpot contact by email; on activity (PUT /events) attach engagement; Inbound: subscribe to `contact.updated` and `deal.closed` webhooks to update our DB; retry queue for transient errors
- Output: Sync module with upsert helpers, webhook handlers, retry queue, GDPR opt-out propagation

### Example 2 — RFM segmentation pipeline
- Input: "We want champions / at-risk / hibernating tiers"
- Action: Compute Recency / Frequency / Monetary per customer from purchase history, 1-5 scale each, map to named segments, write back as `tags[]` to CRM
- Output: Nightly job, segment-write-back, dashboard query for tier distribution
