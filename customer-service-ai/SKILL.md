---
name: customer-service-ai
description: 'AI-powered customer service system builder for chatbots, automated support flows, agent handoff, and multi-channel support. Triggers: "use customer-service-ai", "customer service ai", "customer task".'
allowed-tools: Glob, Grep, Read
---

# Customer Service AI

Build intelligent customer service solutions including chatbots, automated responses, ticket routing, and support analytics.

## Conversation Architecture

Design conversation state to track session, customer, intent, entities, history, sentiment, and channel. This context persistence enables the bot to maintain coherent multi-turn conversations.

### Intent Recognition
Map common customer intents to patterns and actions:
- **Order**: status, cancellation, returns
- **Product**: info, availability
- **Account**: password reset, lockout
- **General**: greeting, thanks, speak-to-agent

Use ML classification when available; fall back to pattern matching. When confidence is below 0.6, ask clarifying questions rather than guessing wrong — a bad answer is worse than asking for clarification.

### Response Generation
Use template-based responses with personalization variables (customer name, order ID, status, tracking URL). Select template variants based on conversation context (new vs returning customer).

## RAG-Enhanced Support

Retrieval Augmented Generation grounds answers in your actual knowledge base, which prevents hallucination about policies, pricing, or product details:

1. Embed knowledge base documents into vector store → verify: step output matches expected outcome
2. On each question, retrieve top 5 similar documents → verify: step output matches expected outcome
3. Build prompt with retrieved context + conversation history → verify: step output matches expected outcome
4. Generate response grounded in actual documentation → verify: output exists + parses without error
5. If no relevant documents found, offer to connect with human agent → verify: step output matches expected outcome

## Ticket Management

Auto-create tickets from conversations with:
- Summary generated from conversation history
- Priority based on sentiment + keywords (urgent/legal = high priority)
- Category from detected intent
- Auto-assignment based on category and agent availability

## Agent Handoff

When the bot can't resolve an issue or the customer requests a human:
1. Create ticket from conversation → verify: output exists + parses without error
2. Find available agent matching the ticket category → verify: step output matches expected outcome
3. Pass full conversation history and customer info to agent → verify: step output matches expected outcome
4. If no agent available, queue for callback with estimated wait time → verify: step output matches expected outcome

Quick escalation is critical — trapping users in a bot loop when they need human help destroys trust.

## Key Metrics

Track: total conversations, tickets by category, avg resolution time, first response time, automation rate (% handled without agent), containment rate (% resolved by bot), CSAT, and NPS.

## Best Practices

1. **Natural conversation** — Friendly, human-like language → verify: step output matches expected outcome
2. **Quick escalation** — Don't trap users with the bot → verify: step output matches expected outcome
3. **Context preservation** — Remember conversation history within session → verify: step output matches expected outcome
4. **Sentiment awareness** — Adjust tone when customer is frustrated → verify: step output matches expected outcome
5. **Feedback collection** — Ask for ratings after resolution → verify: user confirms
6. **Multi-channel consistency** — Same experience across web, mobile, messaging → verify: step output matches expected outcome

## When NOT to use

- High-stakes domains where wrong answers cause harm (medical, legal advice) — escalate to humans
- B2B enterprise support with named CSMs — different model, use `customer-success-manager`
- Internal IT/HR helpdesk — different RAG corpus, different intents
- Pure CRM record updates — use `crm-integration`
- Outbound sales chatbots — different framing, use `outbound-strategist`

## Red Flags

| Thought | Reality |
|---------|---------|
| "Guess intent when confidence < 0.6" | Wrong answers tank CSAT; always clarify under threshold |
| "Lock users in bot until resolved" | Trap = trust collapse; offer human handoff visibly at every step |
| "No RAG, model knows the policy" | Hallucinated policies create legal exposure; ground in your KB |
| "Skip sentiment, just route by category" | Frustrated users need escalation regardless of category |

## Output Contract

Done when:
- Conversation state schema defined (session, customer, intent, entities, history, sentiment, channel)
- Intent classifier returns confidence with clarify-under-0.6 rule
- RAG pipeline grounds answers in actual KB documents
- Agent-handoff flow with full context pass + ticket creation
- Templates allow personalization variables
- Metrics wired: automation rate, containment rate, FRT, CSAT, NPS
- Multi-channel parity tested (web + mobile + messaging)

## Examples

### Example 1 — Order-status bot for ecommerce
- Input: "Build a chatbot for order status + returns + product Qs"
- Action: Define intents (status/return/product/agent), RAG over policy KB, integrate with order API, sentiment monitor escalates angry users, handoff with ticket + full transcript
- Output: Bot with state machine, RAG retrieval, handoff path; containment-rate dashboard

### Example 2 — Adding sentiment-aware escalation
- Input: "Bot keeps frustrating angry customers"
- Action: Add sentiment scoring per turn, drop to human handoff at 2 consecutive negative turns, soften template tone when sentiment < neutral
- Output: Sentiment trigger wired; CSAT improvement measurable in 2 weeks
