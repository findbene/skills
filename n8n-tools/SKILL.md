---
name: n8n-tools
description: 'n8n workflow automation toolkit with 1236 nodes for building, validating, managing, and deploying workflows via MCP. Triggers: "use n8n-tools", "automate n8n tools", "n8n tools".'
allowed-tools: Glob, Grep, Read
---

# n8n Tools

Build, validate, and deploy n8n workflow automations with 1,236 nodes.

## MCP Connection

Package: `n8n-mcp` via npx (docs-only mode by default).
To enable workflow management, add `N8N_API_URL` and `N8N_API_KEY` to the MCP env config in `~/.claude/settings.json`.

## Available Tools

### Core Tools (Always Available — No API Key Needed)

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `tools_documentation` | Get best practices for using n8n-mcp | Start here for guidance |
| `search_nodes` | Search across 1,236 nodes by name/category | Finding the right node for a task |
| `get_node` | Get node properties, docs, examples, versions | Configuring a specific node |
| `validate_node` | Validate node configuration before deployment | Catching errors early |
| `validate_workflow` | Validate complete workflow (connections, expressions, AI tools) | Pre-deployment check |
| `search_templates` | Search 2,709 workflow templates | Finding existing solutions |
| `get_template` | Get complete template workflow JSON | Using a template as starting point |

### Workflow Management (Requires N8N_API_URL + N8N_API_KEY)

| Tool | Purpose |
|------|---------|
| `n8n_create_workflow` | Create a new workflow |
| `n8n_get_workflow` | Get workflow details |
| `n8n_update_full_workflow` | Replace entire workflow |
| `n8n_update_partial_workflow` | Batch update with diff operations |
| `n8n_delete_workflow` | Delete a workflow |
| `n8n_list_workflows` | List all workflows |
| `n8n_validate_workflow` | Validate deployed workflow |
| `n8n_autofix_workflow` | Auto-fix common errors |
| `n8n_deploy_template` | Deploy template directly to instance |
| `n8n_test_workflow` | Test/trigger workflow execution |
| `n8n_executions` | List, get, or delete executions |
| `n8n_health_check` | Check n8n API connectivity |
| `n8n_workflow_versions` | Version history and rollback |

## Workflow Building Process

### 1. Templates First
Always check templates before building from scratch — 2,709 available.

```
search_templates({searchMode: "by_task", task: "webhook_processing"})
search_templates({query: "slack notification"})
search_templates({searchMode: "by_nodes", nodeTypes: ["n8n-nodes-base.slack"]})
search_templates({searchMode: "by_metadata", complexity: "simple", requiredService: "openai"})
```

### 2. Node Discovery
If no suitable template, search for nodes.

```
search_nodes({query: "send email gmail", includeExamples: true})
search_nodes({query: "AI agent langchain"})
search_nodes({query: "scraping", source: "community"})
```

### 3. Node Configuration
Get detailed properties for each node.

```
get_node({nodeType: "n8n-nodes-base.httpRequest", detail: "standard", includeExamples: true})
get_node({nodeType: "n8n-nodes-base.slack", mode: "docs"})
get_node({nodeType: "n8n-nodes-base.httpRequest", mode: "search_properties", propertyQuery: "auth"})
```

### 4. Validation (Multi-Level)

```
// Level 1: Quick required fields check
validate_node({nodeType: "...", config: {...}, mode: "minimal"})

// Level 2: Full validation with runtime profile
validate_node({nodeType: "...", config: {...}, mode: "full", profile: "runtime"})

// Level 3: Complete workflow validation
validate_workflow(workflowJson)
```

### 5. Deploy (If API Configured)

```
n8n_create_workflow(workflow)
n8n_validate_workflow({id: "..."})
n8n_test_workflow({workflowId: "..."})
```

## Popular Nodes Reference

| Node | Type | Use Case |
|------|------|----------|
| `n8n-nodes-base.webhook` | Trigger | Event-driven starts |
| `n8n-nodes-base.scheduleTrigger` | Trigger | Cron/time-based starts |
| `n8n-nodes-base.httpRequest` | Action | API calls |
| `n8n-nodes-base.code` | Action | Custom JS/Python logic |
| `n8n-nodes-base.if` | Logic | Conditional branching |
| `n8n-nodes-base.switch` | Logic | Multi-branch routing |
| `n8n-nodes-base.set` | Transform | Data mapping |
| `n8n-nodes-base.merge` | Transform | Combine data streams |
| `n8n-nodes-base.googleSheets` | Integration | Spreadsheet ops |
| `n8n-nodes-base.slack` | Integration | Messaging |
| `n8n-nodes-base.gmail` | Integration | Email |
| `@n8n/n8n-nodes-langchain.agent` | AI | AI agent workflows |

## Critical Rules

- **Never trust defaults** — explicitly set ALL parameters. Defaults are the #1 source of runtime failures.
- **Templates first** — always search before building from scratch.
- **Validate before deploy** — use multi-level validation (minimal -> full -> workflow).
- **Batch operations** — use `n8n_update_partial_workflow` with multiple operations in one call, not separate calls.
- **Never edit production directly** — copy workflows before AI-assisted editing.

## Adding API Credentials

To enable workflow management, update `~/.claude/settings.json`:

```json
"n8n": {
  "command": "npx",
  "args": ["-y", "n8n-mcp"],
  "env": {
    "MCP_MODE": "stdio",
    "LOG_LEVEL": "error",
    "DISABLE_CONSOLE_OUTPUT": "true",
    "N8N_API_URL": "https://your-n8n-instance.com",
    "N8N_API_KEY": "your-api-key"
  }
}
```

Generate an API key in n8n: Settings > API > API Keys > Create new key.

## When NOT to use

- Task is unrelated to n8n tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | N8N Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for n8n tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving n8n tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
