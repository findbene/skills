---
name: n8n-workflow-patterns
description: 'Production-ready n8n workflow architecture patterns. Trigger: designing an n8n workflow from scratch,. Triggers: "use n8n-workflow-patterns", "automate n8n workflow patterns", "n8n workflow patterns".'
allowed-tools: Glob, Grep, Read
---

# n8n Workflow Patterns

Production-tested architecture patterns for common n8n automation scenarios.

---

## Pattern Selection Guide

| What you are building | Pattern to use |
|---------------------|---------------|
| Receive HTTP data and process it | [Webhook Processing](#webhook-processing) |
| Call external APIs and transform responses | [HTTP API Integration](#http-api-integration) |
| Run on a schedule | [Scheduled Tasks](#scheduled-tasks) |
| Read/write to databases | [Database Operations](#database-operations) |
| AI agent or LLM automation | [AI Agent Workflow](#ai-agent-workflow) |

---

## Webhook Processing

**Trigger:** Webhook node → process → respond

```
Webhook → [Validate Input] → [Process Data] → [Store/Forward] → Respond to Webhook
```

### Node sequence

1. **Webhook** — `httpMethod: POST`, set a unique `path` → verify: step output matches expected outcome
2. **IF** — validate required fields are present (`$json.body.email` exists, etc.) → verify: step output matches expected outcome
3. **Code** — transform/enrich data; access body via `$json.body.field` → verify: step output matches expected outcome
4. **[Action nodes]** — Slack, email, database write, HTTP Request to another API → verify: output file exists + no syntax error
5. **Respond to Webhook** — send back `{"status": "ok"}` or processed result → verify: step output matches expected outcome

### Key rules

- Webhook data is always under `$json.body` — never `$json.field` directly
- Always respond quickly; move heavy processing to a sub-workflow if needed
- Add error handling: IF the required fields are missing, Respond to Webhook with 400 immediately
- Use `responseMode: "lastNode"` to control which node output is the HTTP response

### Minimal example

```javascript
// Webhook node config
{ "httpMethod": "POST", "path": "my-automation", "responseMode": "onReceived" }

// Code node — access webhook payload
const body = $input.first().json.body;
const email = body.email;
const name = body.name;
return [{ json: { email, name, processed: true } }];
```

**Read `references/webhook_processing.md`** for full pattern with auth, error handling, and response formats.

---

## HTTP API Integration

**Pattern:** Trigger → prepare request → HTTP Request → handle response → transform → use

```
[Trigger] → Set (params) → HTTP Request → IF (success check) → Code (transform) → [Next]
                                               ↓ error
                                          [Error Handler]
```

### Key rules

- Always check `statusCode` after HTTP Request — do not assume success
- Store API credentials in n8n Credentials, not hardcoded in expressions
- For paginated APIs: use SplitInBatches with a loop and cursor/page tracking
- Add retry logic for transient failures (rate limits, 5xx errors)

### Response handling pattern

```javascript
// Code node after HTTP Request
const response = $input.first().json;

// Check for API-level errors (not just HTTP errors)
if (response.error || response.status === "error") {
  return [{ json: { success: false, error: response.message } }];
}

// Transform the successful response
return response.data.map(item => ({
  json: {
    id: item.id,
    name: item.name,
    createdAt: item.created_at
  }
}));
```

**Read `references/http_api_integration.md`** for pagination, rate limiting, authentication, and retry patterns.

---

## Scheduled Tasks

**Pattern:** Schedule Trigger → fetch data → process → output

```
Schedule Trigger → [Fetch Data] → [Filter/Transform] → [Action] → [Notify]
```

### Trigger config

```javascript
// Interval-based
{
  "rule": {
    "interval": [{ "field": "hours", "hoursInterval": 1 }]
  }
}

// Cron expression
{ "cronExpression": "0 9 * * 1-5" }  // 9am Mon-Fri
```

### Key rules

- Always add a **Limit** node after the data fetch to avoid processing millions of rows
- For long-running tasks: use sub-workflows to keep the main flow responsive
- Add error notifications (Slack/email) — scheduled tasks fail silently otherwise
- Store last-run timestamps in n8n static data to process only new records

### Incremental processing pattern

```javascript
// Code node — get and update last run time
const staticData = $getWorkflowStaticData("global");
const lastRun = staticData.lastRun || new Date(0).toISOString();
staticData.lastRun = new Date().toISOString();

return [{
  json: {
    fetchSince: lastRun,
    currentRun: staticData.lastRun
  }
}];
```

**Read `references/scheduled_tasks.md`** for full patterns including error notification, incremental sync, and sub-workflow delegation.

---

## Database Operations

**Pattern:** Trigger → validate → read/write DB → transform → respond

```
[Trigger] → [Validate] → [DB Read/Write] → [Transform] → [Output]
```

### Common node choices

| Task | Node |
|------|------|
| PostgreSQL | Postgres node |
| MySQL | MySQL node |
| MongoDB | MongoDB node |
| REST API backed DB | HTTP Request node |
| Simple key-value store | n8n static data |

### Key rules

- Use parameterized queries — never concatenate user input into SQL
- For batch inserts: use SplitInBatches to avoid overloading the DB
- Always handle null results — a DB query that returns 0 rows is not an error
- Use transactions for multi-step operations that must be atomic

### Read and process pattern

```javascript
// Code node after Postgres node (SELECT)
const rows = $input.all();

if (rows.length === 0) {
  return [{ json: { found: false, results: [] } }];
}

return rows.map(row => ({
  json: {
    id: row.json.id,
    status: row.json.status,
    processedAt: new Date().toISOString()
  }
}));
```

**Read `references/database_operations.md`** for batch insert, upsert, transaction, and pagination patterns.

---

## AI Agent Workflow

**Pattern:** Input → AI Agent → tool calls → response processing → output

```
[Input] → AI Agent → [Tools: search, calc, DB lookup] → Code (parse) → [Output]
```

### n8n AI Agent node setup

```javascript
{
  "agent": "conversationalAgent",
  "text": "={{ $json.userMessage }}",
  "options": {
    "systemMessage": "You are a helpful assistant...",
    "maxIterations": 10,
    "returnIntermediateSteps": false
  }
}
```

### Key rules

- Use `returnIntermediateSteps: true` during development to debug tool calls
- Limit `maxIterations` to prevent runaway loops (10 is usually sufficient)
- Always parse AI output with a Code node before using it downstream — LLMs return text, not structured data
- Add a fallback branch: if AI output is unparseable, handle gracefully

### Output parsing pattern

```javascript
// Code node — parse AI response
const aiOutput = $input.first().json.output;

// Try to extract JSON from the response
try {
  const jsonMatch = aiOutput.match(/\{[\s\S]*\}/);
  if (jsonMatch) {
    const parsed = JSON.parse(jsonMatch[0]);
    return [{ json: { success: true, data: parsed } }];
  }
} catch(e) {}

// Fallback: return raw text
return [{ json: { success: true, data: { text: aiOutput } } }];
```

**Read `references/ai_agent_workflow.md`** for tool configuration, memory patterns, multi-agent orchestration, and RAG integration.

---

## Universal Best Practices

**Error handling placement:** Put IF error-check nodes immediately after each external call (HTTP Request, DB query, AI Agent). Do not let errors propagate silently.

**Data validation:** Validate inputs at the entry point (Webhook or Schedule) before any processing. Fail fast with a clear error message.

**Node naming:** Name nodes for what they do, not what they are. "Fetch Active Users" is better than "Postgres 2".

**Limit large datasets:** Always add a **Limit** node when fetching from a database or API that might return unlimited rows.

**Testing:** Use n8n "Execute Node" feature to test individual nodes before running the full workflow.

## When NOT to use

- Task is unrelated to n8n workflow patterns — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | N8N Workflow Patterns needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for n8n workflow patterns
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving n8n workflow patterns
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
