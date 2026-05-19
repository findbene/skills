---
name: n8n-node-configuration
description: "Operation-aware node configuration for n8n. Trigger: configuring node parameters, understanding required vs optional fields, working with displayOptions property dependencies, choosing how much detai."
allowed-tools: Glob, Grep, Read
---

# n8n Node Configuration

Operation-aware configuration guide — the right fields depend on the operation you choose.

---

## Core Concept: Operations Change Required Fields

This is the most important thing to understand: the required fields for a node depend on which `resource` and `operation` you select. Configuring without knowing this leads to broken workflows.

```javascript
// Slack node — "post" operation
{
  "resource": "message",
  "operation": "post",
  "channel": "#general",   // required for post
  "text": "Hello!"         // required for post
}

// Slack node — "update" operation
{
  "resource": "message",
  "operation": "update",
  "messageId": "123",      // required for update (different field!)
  "text": "Updated!"       // required for update
  // channel NOT required for update
}
```

---

## Discovery Workflow

Always follow this sequence when configuring an unfamiliar node:

```
1. search_nodes({query: "node name"})          → get nodeType
2. get_node({nodeType, detail: "standard"})    → see operations + required fields
3. validate_node({nodeType, config, mode: "minimal"}) → verify required fields
4. [Fix issues] → verify: diff matches intended change
5. validate_node({nodeType, config, profile: "runtime"}) → full validation
```

**`get_node` detail levels:**
- `"standard"` (default) — covers 95% of use cases, includes operations + common fields
- `"full"` — complete schema with all properties and displayOptions
- `"minimal"` — just node name and description
- `"docs"` — human-readable markdown

Start with `"standard"`. Only escalate to `"full"` if you need to understand complex property dependencies.

---

## Property Dependencies (displayOptions)

Many fields only appear when other fields have specific values. This is why you can't configure by guessing.

```javascript
// HTTP Request node
// When method='GET': no body fields shown
// When method='POST': sendBody, bodyType, body appear

// IF node
// When operation='larger': value2 is required
// When operation='exists': value2 is NOT shown (not required)

// Airtable node
// resource='record' + operation='create': fields is required
// resource='record' + operation='search': filterByFormula is optional
```

When `validate_node` returns `"field not shown for current settings"`, you have a displayOptions mismatch — the field you're setting isn't valid for your chosen resource/operation combination.

---

## Common Node Patterns

### HTTP Request

```javascript
{
  "method": "GET",            // GET, POST, PUT, PATCH, DELETE
  "url": "https://api.example.com/endpoint",
  "authentication": "none",   // or "predefinedCredentialType"
  // For POST/PUT — add:
  "sendBody": true,
  "bodyType": "json",         // json, form, formData, raw
  "body": { "key": "value" }
}
```

### Webhook

```javascript
{
  "httpMethod": "POST",       // GET, POST, PUT, etc.
  "path": "my-webhook",       // unique path — no dynamic expressions here
  "responseMode": "onReceived"
}
```

### Slack (message)

```javascript
// Post message
{
  "resource": "message",
  "operation": "post",
  "channel": "#general",
  "text": "Hello world",
  "otherOptions": {}
}
```

### Code Node

```javascript
{
  "language": "javaScript",  // or "python"
  "jsCode": "// your code\nreturn $input.all();"
}
```

### Set Node (field mapping)

```javascript
{
  "mode": "manual",
  "duplicateItem": false,
  "assignments": {
    "assignments": [
      { "id": "field1", "name": "outputField", "value": "={{ $json.inputField }}", "type": "string" }
    ]
  }
}
```

### IF Node

```javascript
{
  "conditions": {
    "options": { "caseSensitive": true, "leftValue": "" },
    "conditions": [
      {
        "leftValue": "={{ $json.status }}",
        "rightValue": "active",
        "operator": { "type": "string", "operation": "equals" }
      }
    ],
    "combinator": "and"
  }
}
```

---

## patchNodeField vs Full Update

Use `patchNodeField` (via `n8n_update_partial_workflow`) for surgical edits to a single field — never rewrite the whole node config when you only need to change one thing.

```javascript
// ✅ Surgical edit
{ type: "updateNodeField", nodeName: "HTTP Request", field: "url", value: "https://new-url.com" }

// ❌ Unnecessary — don't replace entire node config to change one field
```

---

## Validation Reference

| Error | Meaning | Fix |
|-------|---------|-----|
| `"required field missing"` | Field needed for this operation | Check `get_node` for the operation's required fields |
| `"field not shown for current settings"` | displayOptions mismatch | Wrong resource/operation combination |
| `"invalid value"` | Value type mismatch | Check expected type (string vs number vs boolean) |
| `"credential not found"` | Credential ID doesn't exist | Create credential first |

---

## Reference Files

- **`references/OPERATION_PATTERNS.md`** — Detailed configuration examples for 15+ common nodes (Slack, Gmail, Airtable, Google Sheets, Postgres, HTTP Request, etc.) organized by resource+operation
- **`references/DEPENDENCIES.md`** — How displayOptions work, reading the schema to predict which fields appear for which operations, and debugging dependency mismatches

## When NOT to use

- Task is unrelated to n8n node configuration — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | N8N Node Configuration needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for n8n node configuration
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving n8n node configuration
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
