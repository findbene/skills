---
name: n8n
description: "n8n workflow automation expert for designing, building, debugging, and optimizing n8n pipelines with 400+ integrations. Triggers: 'use n8n', 'n8n', 'n8n task'."
allowed-tools: Glob, Grep, Read
version: 1.0.0
triggers:
  - n8n workflow
  - n8n automation
  - build automation
  - workflow builder
  - n8n node
  - n8n webhook
  - n8n expression
  - n8n self-hosted
  - automate with n8n
  - n8n error
references:
  - references/api-reference.md
  - references/custom-nodes-reference.md
  - references/workflow-reference.md
---

# n8n Skill

## Purpose
Design, build, debug, and optimize n8n automation workflows for any use case — from simple webhook-to-action pipelines to complex multi-branch agent orchestration with error handling.

## Activation
Load this skill when the user asks about:
- Building n8n workflows for any automation use case
- Configuring specific n8n nodes (HTTP Request, Webhook, Function, If, Switch, etc.)
- n8n expressions and variable references
- Error handling, retry logic, and fault tolerance in n8n
- The n8n REST API for programmatic workflow management
- Building custom n8n nodes
- Integrating n8n with external services (Supabase, Slack, APIs)
- Migrating from Zapier or Make (Integromat) to n8n

## Core Concepts

### Workflow Structure
Every n8n workflow has:
- **Trigger node** — starts the workflow (Webhook, Schedule, Event)
- **Processing nodes** — transform, filter, enrich data
- **Action nodes** — write to databases, call APIs, send messages
- **Error workflow** — separate workflow that handles failures

### Data Model
n8n passes data between nodes as arrays of items:
```json
[
  { "json": { "name": "Alice", "score": 95 } },
  { "json": { "name": "Bob", "score": 72 } }
]
```
Each node receives the full array and outputs a (possibly transformed) array.

### Expressions
Reference data from previous nodes using `{{ }}` syntax:
```
{{ $json.fieldName }}                    -- current item's field
{{ $node["NodeName"].json.fieldName }}   -- specific node's output
{{ $items("NodeName")[0].json.field }}   -- first item from a node
{{ $now.toISO() }}                       -- current timestamp
{{ $env.MY_ENV_VAR }}                    -- environment variable
```

## Workflow Design Principles

### 1. One Trigger, One Purpose
Each workflow should have a clear, single trigger. Don't try to handle 10 different triggers in one workflow — create separate workflows and use the "Execute Workflow" node to share logic.

### 2. Always Configure Error Workflows
```
Workflow Settings -> Error Workflow -> point to a dedicated error handler
```
The error handler receives: `{{ $json.execution.error.message }}` and full execution context.

### 3. Use "Set" Node to Clean Data
Before passing data to external APIs, use a Set node to:
- Rename fields to match API expectations
- Remove unnecessary fields (reduce payload size)
- Cast types (string to number)

### 4. Test With Static Data
Pin output data on any node during development: right-click node -> "Pin Data". This lets you develop downstream logic without re-triggering expensive API calls.

### 5. Use Sub-workflows for Reusable Logic
```
Main Workflow
  -> Execute Workflow: "Enrich Lead"
  -> Execute Workflow: "Send Notification"
  -> Execute Workflow: "Log to Supabase"
```

## Common Workflow Patterns (Quick Reference)

### Webhook -> Process -> Respond
```
Webhook (POST) -> Function (transform) -> HTTP Request (external API) -> Respond to Webhook
```

### Schedule -> Fetch -> Filter -> Act
```
Schedule (cron) -> HTTP Request (fetch data) -> IF (filter condition) -> [Yes: write to DB] [No: do nothing]
```

### Error Handling Pattern
```
Main Workflow (error workflow set)
  -> ... nodes ...
  -> Error handler workflow:
      Webhook Error Trigger -> Set (format error) -> Slack (notify) -> Supabase (log error)
```

### Fan-Out / Fan-In
```
Get Items -> SplitInBatches -> [process each batch] -> Merge -> Aggregate Results
```

## Integration with This Project (AI Agency)
n8n is the **primary workflow automation layer** for the AI Agency project. Use it for:
- Triggering agent pipelines on schedule (Niche Architect at 6am daily)
- Webhooks that receive Publisher Agent completion events
- Error alerts to Slack/email when Agent Auditor scores drop below threshold
- Lead qualification notifications to Biniyam
- Supabase write operations for pipeline run logs

## Triggers

\\\"n8n\\\", \\\"n8n workflow\\\", \\\"automation workflow\\\", \\\"n8n node\\\", \\\"workflow builder\\\", \\\"n8n trigger\\\", \\\"webhook workflow\\\", \\\"n8n expression\\\", \\\"workflow error\\\", \\\"n8n credential\\\", \\\"n8n REST API\\\", \\\"custom n8n no

## When NOT to use

- Task is unrelated to n8n — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | N8N needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for n8n
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving n8n
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
