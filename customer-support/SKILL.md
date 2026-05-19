---
name: customer-support
description: "AI-powered customer support system implementation for RAG-based knowledge retrieval, ticket handling, and Supabase pgvect. Triggers: 'use customer-support', 'customer support', 'customer-support task."
allowed-tools: Glob, Grep, Read
version: 1.0.0
triggers:
  - customer support
  - support ticket
  - help desk
  - customer service
  - ticket routing
  - escalation
  - knowledge base
  - support automation
  - RAG support
  - chatbot support
  - customer query
  - support workflow
references:
  - references/escalation-guide.md
  - references/response-templates.md
---

# Customer Support Skill

## Purpose
Design and implement AI-powered customer support systems — from FAQ bots to full ticket routing and escalation pipelines with human handoff.

## Activation
Load this skill when the user asks about:
- Building a customer support chatbot or virtual agent
- Designing ticket routing and prioritization logic
- Creating escalation workflows (bot → human)
- Writing support response templates
- Implementing RAG-based knowledge base search
- Setting up SLA tracking and breach alerts
- Customer Support Agent implementation (Priority #1 build for this project)

## System Architecture

### Tiered Support Model
```
Tier 0 (Self-service): FAQ page, documentation, knowledge base
Tier 1 (AI Agent): Handles 70-80% of queries autonomously
Tier 2 (Human Agent): Complex, sensitive, or high-value issues
Tier 3 (Specialist): Billing disputes, legal, account issues
```

### Support Agent Pipeline
```
Incoming query
  → Classify intent (FAQ / billing / technical / complaint / escalation)
  → Route to appropriate tier
  → If Tier 1:
      → Search knowledge base (RAG)
      → Generate response
      → Check confidence score
      → If confidence < threshold → escalate
      → If confidence >= threshold → send response
  → Log interaction to Supabase
```

## Customer Support Agent (LangGraph — Priority #1)

```python
from langgraph.graph import StateGraph, END
from typing import TypedDict, Optional

class SupportState(TypedDict):
    session_id: str
    customer_id: str
    query: str
    intent: str
    retrieved_docs: list[dict]
    response: str
    confidence: float
    escalated: bool
    ticket_id: Optional[str]

def build_support_graph():
    graph = StateGraph(SupportState)

    graph.add_node("classify_intent", classify_intent_node)
    graph.add_node("retrieve_context", rag_retrieval_node)
    graph.add_node("generate_response", response_generation_node)
    graph.add_node("check_confidence", confidence_check_node)
    graph.add_node("escalate", escalation_node)
    graph.add_node("send_response", send_response_node)

    graph.set_entry_point("classify_intent")
    graph.add_edge("classify_intent", "retrieve_context")
    graph.add_edge("retrieve_context", "generate_response")
    graph.add_edge("generate_response", "check_confidence")
    graph.add_conditional_edges(
        "check_confidence",
        lambda state: "escalate" if state["escalated"] else "send_response",
        {"escalate": "escalate", "send_response": "send_response"}
    )
    graph.add_edge("escalate", END)
    graph.add_edge("send_response", END)

    return graph.compile()
```

## RAG Knowledge Base (Supabase pgvector)

```python
from openai import OpenAI
import psycopg2
import json

client = OpenAI()

def embed_text(text: str) -> list[float]:
    response = client.embeddings.create(model="text-embedding-3-small", input=text)
    return response.data[0].embedding

def search_knowledge_base(query: str, limit: int = 5) -> list[dict]:
    query_embedding = embed_text(query)
    conn = psycopg2.connect(SUPABASE_DB_URL)
    cursor = conn.cursor()
    cursor.execute("""
        SELECT title, content, category,
               1 - (embedding <=> %s::vector) AS similarity
        FROM knowledge_base
        WHERE 1 - (embedding <=> %s::vector) > 0.75
        ORDER BY embedding <=> %s::vector
        LIMIT %s
    """, (json.dumps(query_embedding),) * 3 + (limit,))
    return [
        {"title": r[0], "content": r[1], "category": r[2], "similarity": r[3]}
        for r in cursor.fetchall()
    ]
```

## Intent Classification

```python
SUPPORT_INTENTS = {
    "billing": ["invoice", "payment", "charge", "refund", "pricing", "subscription"],
    "technical": ["error", "bug", "broken", "not working", "crash", "integration"],
    "account": ["password", "login", "access", "account", "delete", "cancel"],
    "general_faq": ["how do I", "what is", "can I", "does it"],
    "complaint": ["frustrated", "angry", "terrible", "worst"],
    "escalation": ["speak to human", "manager", "supervisor", "legal"],
}

ESCALATION_INTENTS = {"escalation", "complaint"}
CONFIDENCE_THRESHOLD = 0.75
```

## SLA Targets

```python
SLA_TARGETS = {
    "critical": {"first_response": 1, "resolution": 4},    # hours
    "high":     {"first_response": 4, "resolution": 24},
    "medium":   {"first_response": 8, "resolution": 48},
    "low":      {"first_response": 24, "resolution": 72},
}

def get_ticket_priority(intent: str, customer_tier: str) -> str:
    if intent in ESCALATION_INTENTS or customer_tier == "enterprise":
        return "critical"
    elif intent == "billing":
        return "high"
    elif intent == "technical":
        return "medium"
    return "low"
```

## Triggers

\\\"customer support\\\", \\\"support system\\\", \\\"knowledge base\\\", \\\"support ticket\\\", \\\"RAG support\\\", \\\"support agent\\\", \\\"ticket handling\\\", \\\"support workflow\\\", \\\"FAQ automation\\\", \\\"suppo...

## When NOT to use

- Task is unrelated to customer support — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Customer Support needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for customer support
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving customer support
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
