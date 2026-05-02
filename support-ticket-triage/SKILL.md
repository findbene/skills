---
name: support-ticket-triage
description: >
  Triages, categorizes, prioritizes, and drafts responses for customer support
  tickets — individually or in batches. Use this skill whenever the user has
  support tickets to process, wants to categorize or route tickets, needs to
  draft customer replies, wants to build a triage SOP, or is setting up a
  support automation workflow. Also trigger when they mention inbox overload,
  slow response times, or want to train an AI on their support patterns.
---

# Support Ticket Triage

You process support tickets with the speed and consistency of a trained support ops
team. The goal is: right priority, right category, right response — fast.

## What you need

If not provided, ask:

- **The ticket(s)** — paste them in, share a CSV, or describe the type of tickets
- **Product / service context** — what does the company offer? What are common issues?
- **Tone guidelines** — formal, friendly, brand-specific language preferences?
- **SLA expectations** — if known (e.g., "P1 = respond within 1 hour")
- **Escalation rules** — what triggers a human handoff vs. automated response?

A single ticket or a batch of 20+ both work. Scale your output accordingly.

## Triage framework

### Step 1: Categorize

Assign each ticket to one primary category and one optional sub-category.

**Standard categories:**
- `billing` — payments, invoices, charges, refunds
- `account` — login, access, permissions, password
- `bug` — something isn't working as documented
- `feature-request` — user wants something new
- `how-to` — user doesn't know how to do something (product is working fine)
- `onboarding` — new user setup friction
- `cancellation` — churn signal, explicit or implied
- `abuse/fraud` — policy violation, suspicious activity
- `compliment` — positive feedback (don't ignore these — they're conversion assets)

If the category doesn't fit, name it. Don't force-fit.

### Step 2: Prioritize

**P1 — Critical (respond ASAP)**
- Revenue impact (billing errors, broken paid features)
- Security concern (account compromise, data exposure)
- Multiple users affected (system-wide issue)
- Explicit churn signal ("I'm canceling unless...")

**P2 — High (respond within 1 business day)**
- Single-user bug blocking core workflow
- Onboarding blocker (new customer stuck in setup)
- Billing question (not necessarily an error, but money-related)

**P3 — Normal (respond within 2–3 business days)**
- How-to questions with documented answers
- Feature requests
- General feedback

**P4 — Low (batch/async OK)**
- Compliments
- Duplicate tickets
- Out-of-scope requests

### Step 3: Sentiment flag

Tag sentiment: `frustrated` / `neutral` / `happy` / `urgent-tone`

Frustrated + P1 = lead with empathy before solution. Never start with "Have you
tried turning it off and on again" energy for an angry customer.

### Step 4: Draft response

Write the response. Follow this structure:

1. **Acknowledgment** — one sentence showing you read their actual message, not a
   template opener. ("I can see the payment went through twice on Tuesday — that's
   frustrating and I want to fix it now.")
2. **Answer or next step** — clear, direct. If you don't know the answer, say what
   you're doing to find out and when they'll hear back.
3. **Confirmation ask** — only if needed. Don't ask "does this help?" — it adds
   friction. Only ask if you genuinely need their input to proceed.
4. **Warm close** — one line. Keep it human, not corporate.

## Batch output format

When processing multiple tickets, output a triage table first, then draft responses:

### Triage Summary

| # | Subject / snippet | Category | Priority | Sentiment | Response needed? |
|---|-------------------|----------|----------|-----------|-----------------|
| 1 | ... | billing | P1 | frustrated | Yes — draft below |
| 2 | ... | how-to | P3 | neutral | Yes — draft below |

### Drafted Responses

**Ticket #1**
> [Original ticket text — truncated if long]

**Response:**
[Draft response here]

---

## Escalation signals

Flag these for human review — don't draft a response:
- Legal threats or mentions of lawyers
- Data breach or PII exposure concerns
- Media / press inquiries
- Repeat contacts (3+ tickets on same issue = systemic problem)
- Any ticket where you're less than 85% confident in the answer

## SOP generation mode

If the user wants to build a triage SOP rather than process specific tickets, switch
to structured documentation mode. Produce:
1. Categorization guide with definitions and examples
2. Priority decision tree (flowchart in text/Mermaid format)
3. Response templates for the top 5–10 ticket types
4. Escalation rules with clear owners
5. Metrics to track (first response time, resolution time, CSAT)
