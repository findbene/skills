---
name: n8n-mcp-tools-expert
description: 'Master guide for using n8n-mcp MCP server tools. Always use this skill before calling any n8n-mcp tool —. Triggers: "use n8n-mcp-tools-expert", "automate n8n mcp tools expert", "n8n mcp tools expert".'
allowed-tools: Glob, Grep, Read
---

# n8n MCP Tools Expert

Master guide for using n8n-mcp tools correctly and efficiently.

---

## Tool Categories

| Category | Key Tools |
|----------|-----------|
| Node Discovery | `search_nodes`, `get_node` |
| Configuration Validation | `validate_node`, `validate_workflow`, `n8n_autofix_workflow` |
| Workflow Management | `n8n_create_workflow`, `n8n_update_partial_workflow`, `n8n_get_workflow` |
| Templates | `n8n_deploy_template`, `n8n_search_templates` |
| Credentials | `n8n_manage_credentials` |
| Data Tables | `n8n_manage_datatable` |
| Security Audit | `n8n_audit_instance` |

---

## Most Used Tools

| Tool | Use When | Typical Speed |
|------|----------|--------------|
| `search_nodes` | Finding nodes by keyword | <20ms |
| `get_node` | Understanding node operations | <10ms |
| `validate_node` | Checking configuration | <100ms |
| `n8n_update_partial_workflow` | Editing workflows (most common!) | 50-200ms |
| `n8n_create_workflow` | Creating new workflows | 100-500ms |
| `validate_workflow` | Checking full workflow | 100-500ms |
| `n8n_deploy_template` | Deploy from template library | 200-500ms |
| `n8n_audit_instance` | Security audit | 500-5000ms |

---

## Standard Workflows

### Finding and Configuring a Node

```
1. search_nodes({query: "slack"}) → verify: step output matches expected outcome
   → Returns: nodes-base.slack

2. get_node({nodeType: "nodes-base.slack"}) → verify: step output matches expected outcome
   → Returns: operations, required fields, examples

3. [Optional] get_node({nodeType: "nodes-base.slack", mode: "docs"}) → verify: step output matches expected outcome
   → Returns: readable markdown documentation
```

**`get_node` detail levels:**
- `"standard"` (default) — best for 95% of cases, 1-2K tokens
- `"minimal"` — just the node name and description
- `"full"` — complete schema, all properties (use sparingly)
- `"docs"` — human-readable markdown

### Validating a Configuration

```
1. validate_node({nodeType, config: {}, mode: "minimal"}) → verify: step output matches expected outcome
   → Check required fields only

2. validate_node({nodeType, config, profile: "runtime"}) → verify: command exit code 0
   → Full validation including dependent fields

3. Fix errors → validate again
```

### Creating a Workflow

```
1. n8n_create_workflow({name, nodes, connections}) → verify: output file exists + no syntax error
2. validate_workflow({id}) → verify: step output matches expected outcome
3. n8n_update_partial_workflow({id, operations: [...]}) → verify: step output matches expected outcome
4. validate_workflow({id})   ← always validate after edits → verify: step output matches expected outcome
5. n8n_update_partial_workflow({id, operations: [{type: "activateWorkflow"}]}) → verify: step output matches expected outcome
```

### Editing an Existing Workflow (Most Common)

Use `n8n_update_partial_workflow` for surgical edits — never rewrite the whole workflow:

```javascript
// Add a node
{ type: "addNode", node: { ... } }

// Update a field
{ type: "updateNodeField", nodeName: "HTTP Request", field: "url", value: "https://..." }

// Connect nodes
{ type: "addConnection", source: "Webhook", target: "Code" }

// Activate
{ type: "activateWorkflow" }
```

---

## nodeType Format

```javascript
// ✅ Correct format
"nodes-base.slack"
"nodes-base.httpRequest"
"nodes-base.code"
"nodes-base.set"
"nodes-base.if"

// ❌ Wrong formats
"slack"                  // missing prefix
"n8n-nodes-base.slack"  // wrong prefix
"Slack"                  // wrong case
```

---

## Key Parameter Rules

**`validate_node` modes:**
- `mode: "minimal"` — check required fields only (fast)
- `mode: "full"` — validate all properties and dependencies
- `profile: "runtime"` — validate as if actually running

**`get_node` modes:**
- Default to `detail: "standard"` — covers 95% of use cases
- Only use `detail: "full"` when you need the complete property schema

**Connections format:**
```javascript
{
  "NodeA": {
    "main": [[{ "node": "NodeB", "type": "main", "index": 0 }]]
  }
}
```

---

## Credential Management

```javascript
// List available credentials
n8n_manage_credentials({ action: "list" })

// Get credential schema for a type
n8n_manage_credentials({ action: "getSchema", credentialType: "slackApi" })

// Create credential
n8n_manage_credentials({
  action: "create",
  name: "My Slack",
  type: "slackApi",
  data: { accessToken: "xoxb-..." }
})
```

---

## Template Library

2,700+ real community workflows available:

```javascript
// Search templates
n8n_search_templates({ query: "slack notification", limit: 10 })

// Deploy a template to your instance
n8n_deploy_template({ templateId: 1234 })
```

---

## Security Audit

```javascript
// Standard audit
n8n_audit_instance({})

// Deep scan with custom checks
n8n_audit_instance({
  additionalOptions: {
    daysAbandonedWorkflow: 90,
    categories: ["credentials", "nodes", "instance"]
  }
})
```

---

## Reference Files

Read these for deeper detail on specific workflows:

- **`references/SEARCH_GUIDE.md`** — Advanced search patterns, filter by category/package, handling ambiguous results
- **`references/VALIDATION_GUIDE.md`** — Full validation profiles, error codes, auto-fix patterns, displayOptions dependencies
- **`references/WORKFLOW_GUIDE.md`** — Workflow JSON structure, connection format, partial update operations, batch editing patterns

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Using `"slack"` instead of `"nodes-base.slack"` | Always include `nodes-base.` prefix |
| Calling `$('Node').json` in Code node | Use `$('Node').first().json` |
| Rewriting entire workflow instead of patching | Use `n8n_update_partial_workflow` |
| Skipping validation after edit | Always validate before activating |
| `detail: "full"` for every `get_node` | Use `"standard"` — it's 95% of what you need |

## When NOT to use

- Task is unrelated to n8n mcp tools expert — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | N8N Mcp Tools Expert needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for n8n mcp tools expert
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving n8n mcp tools expert
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
