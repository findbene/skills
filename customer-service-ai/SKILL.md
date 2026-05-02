---
name: customer-service-ai
description: "AI-powered customer service system builder for chatbots, automated support flows, agent handoff, and multi-channel support automation. Use this skill any time an AI customer service system needs to be built, chatbots need to be created, automated support flows need to be designed, or AI-to-human handoffs need to be implemented. Trigger immediately on: \"customer service AI\", \"support chatbot\", \"AI chatbot\", \"automated support\", \"customer service bot\", \"support flow\", \"agent handoff\", \"FAQ bot\", \"ticket automation\", \"support automation\", \"live chat AI\", \"customer service system\". If someone says \"build a customer service chatbot\" or \"automate our support\" this skill MUST trigger."
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

1. Embed knowledge base documents into vector store
2. On each question, retrieve top 5 similar documents
3. Build prompt with retrieved context + conversation history
4. Generate response grounded in actual documentation
5. If no relevant documents found, offer to connect with human agent

## Ticket Management

Auto-create tickets from conversations with:
- Summary generated from conversation history
- Priority based on sentiment + keywords (urgent/legal = high priority)
- Category from detected intent
- Auto-assignment based on category and agent availability

## Agent Handoff

When the bot can't resolve an issue or the customer requests a human:
1. Create ticket from conversation
2. Find available agent matching the ticket category
3. Pass full conversation history and customer info to agent
4. If no agent available, queue for callback with estimated wait time

Quick escalation is critical — trapping users in a bot loop when they need human help destroys trust.

## Key Metrics

Track: total conversations, tickets by category, avg resolution time, first response time, automation rate (% handled without agent), containment rate (% resolved by bot), CSAT, and NPS.

## Best Practices

1. **Natural conversation** — Friendly, human-like language
2. **Quick escalation** — Don't trap users with the bot
3. **Context preservation** — Remember conversation history within session
4. **Sentiment awareness** — Adjust tone when customer is frustrated
5. **Feedback collection** — Ask for ratings after resolution
6. **Multi-channel consistency** — Same experience across web, mobile, messaging
