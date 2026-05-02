---
name: hubspot-sales-brief
description: >
  Creates structured sales briefs, deal summaries, contact notes, and follow-up
  sequences formatted for HubSpot CRM — including deal properties, contact context,
  next steps, and email sequences ready to copy into HubSpot. Use this skill
  whenever the user wants to document a sales conversation, prepare a deal brief,
  write follow-up emails for a prospect, summarize a discovery call, or organize
  their sales pipeline context in HubSpot. Also trigger when they mention deals,
  prospects, pipelines, CRM notes, or follow-up sequences.
---

# HubSpot Sales Brief

You turn sales conversations, prospect research, and deal context into structured
HubSpot-ready documents — saving reps time and ensuring nothing falls through the
cracks between calls.

## Inputs to collect

If not already provided:

- **Prospect / company name** — who is the deal with?
- **Contact name and role** — who did you speak to?
- **Source** — how did this lead come in? (inbound, outbound, referral, event)
- **Conversation summary** — what was discussed? (rough notes, transcript, or verbal summary)
- **Pain points identified** — what problems did they share?
- **Budget / authority / need / timeline (BANT)** — any of these known?
- **Next step agreed** — what was the outcome of the last touchpoint?
- **Deal stage** — where are they in your pipeline?

Even rough notes ("we talked for 30 min, they're a 50-person company struggling with X")
are enough to produce a solid brief.

## Output package

---

## HubSpot Deal Brief: [Company Name]
*Created: [date] | Deal owner: [name if provided]*

### Contact summary

| Field | Value |
|-------|-------|
| Company | [name] |
| Contact | [name, title] |
| Email | [if known] |
| Phone | [if known] |
| Company size | [employees / revenue tier if known] |
| Industry | [sector] |
| Lead source | [inbound / outbound / referral / etc.] |

### Deal properties (HubSpot fields)

| Property | Value |
|----------|-------|
| Deal name | [Company] — [Service/Product] |
| Deal stage | [Appointment Scheduled / Qualified / Proposal Sent / etc.] |
| Amount | [$ estimate or TBD] |
| Close date | [expected close — YYYY-MM-DD or "Q[X] 2026"] |
| Pipeline | [Sales / Enterprise / SMB — whichever applies] |

### Discovery summary

**Problem they're trying to solve:**
[2–3 sentences. Use their language, not yours. This is what they said the pain is.]

**Why now:**
[What triggered this conversation? Budget cycle, recent event, growth pressure, etc.]

**Current solution / status quo:**
[What are they using or doing today? Why isn't it working?]

**BANT snapshot:**
- Budget: [confirmed / estimated / unknown — and amount if known]
- Authority: [decision-maker / influencer / gatekeeper — and who holds final say]
- Need: [explicit / latent — and urgency level]
- Timeline: [stated timeline or implied]

**Buying signals:**
[Any positive indicators: asked about pricing, mentioned a deadline, compared you
to a competitor, referenced internal approval process]

**Objections / concerns raised:**
[What hesitations or blockers came up? Note them here for follow-up handling]

### Next steps

| Action | Owner | Due date |
|--------|-------|----------|
| [specific action] | [rep / prospect] | [date] |

Be concrete. "Follow up" is not a next step. "Send proposal by Thursday EOD" is.

### Internal notes
Anything that shouldn't go in the customer-facing follow-up but is relevant to the
deal: competitor mentions, internal politics at the prospect, budget cycle information,
personal context (they're moving offices, going on vacation, etc.).

---

## Follow-up email sequence

Write 2–3 follow-up emails based on the deal stage and conversation. Label each:

**Email 1 — [Send timing, e.g., "Same day / next morning"]**
Subject: [subject line]

[Body — 3–5 sentences max. Reference something specific from the conversation.
Restate the agreed next step. Single CTA.]

**Email 2 — [Timing, e.g., "3 days later if no reply"]**
Subject: Re: [original subject or new angle]

[Shorter — 2–3 sentences. Add value (a relevant resource, insight, or question)
rather than just nudging.]

**Email 3 — [Timing, e.g., "7 days later — breakup email"]**
Subject: [Permission to close the loop?]

[1–2 sentences. Acknowledge they may have moved on. Low-pressure close or
re-open door for a future conversation.]

---

## HubSpot paste guide

**Contact notes field:** Copy the Discovery Summary section. Keep it under 500 chars
for the preview to display cleanly in list view.

**Deal description:** Copy the Problem + Why Now sentences.

**Tasks:** Create one task per row in the Next Steps table.

**Email sequences:** If using HubSpot Sequences, the three emails above map to
sequence steps 1, 2, and 3. Set enrollment triggers based on deal stage.

## Pipeline stage definitions (customize to your process)

| Stage | Definition | Typical next action |
|-------|------------|---------------------|
| New Lead | Initial contact, not yet qualified | Discovery call |
| Appointment Scheduled | Call/meeting booked | Prepare discovery framework |
| Qualified | BANT partially confirmed | Proposal or demo |
| Proposal Sent | Offer delivered | Follow-up sequence |
| Negotiation | Pricing / scope discussion | Close call |
| Closed Won | Signed | Kickoff / onboarding |
| Closed Lost | Not proceeding | Loss reason logged |
