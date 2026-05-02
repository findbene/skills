---
name: n8n-node-configuration
description: Operation-aware node configuration for n8n. Always use this skill when configuring node parameters, understanding required vs optional fields, working with displayOptions property dependencies, choosing how much detail to fetch with get_node, or setting up any node that has multiple resources/operations (Slack, HTTP Request, Airtable, Google Sheets, etc.). Different operations require completely different fields — guessing will produce broken configs. Load this skill whenever you're about to configure a node you haven't configured before, or when a validation error says a field is missing.
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
4. [Fix issues]
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
