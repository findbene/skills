---
name: n8n-validation-expert
description: Validate and auto-fix n8n workflow configurations. Always use this skill when running validate_node or validate_workflow, interpreting validation errors, deciding whether to fix or ignore a validation message, working around false positives, or using n8n_autofix_workflow. Validation errors in n8n are often non-obvious — what looks like an error can be a false positive, and what looks minor can break the workflow. Load this skill whenever validation is part of the task.
---

# n8n Validation Expert

Expert guide for interpreting and fixing n8n validation errors.

---

## Validation Tools

| Tool | Use When | Mode |
|------|----------|------|
| `validate_node` | Check a single node config | `mode: "minimal"` or `profile: "runtime"` |
| `validate_workflow` | Check entire workflow JSON | — |
| `n8n_autofix_workflow` | Auto-fix common issues | — |

**Standard validation sequence:**
```
1. validate_node({nodeType, config, mode: "minimal"})   → check required fields
2. [Fix issues]
3. validate_node({nodeType, config, profile: "runtime"}) → full runtime check
4. n8n_create_workflow or n8n_update_partial_workflow
5. validate_workflow({id})                               → end-to-end check
6. [Fix] → validate_workflow again
7. Activate
```

---

## Validation Modes

### validate_node modes

| Mode | What It Checks | When to Use |
|------|---------------|-------------|
| `mode: "minimal"` | Required fields only | Quick check before building |
| `mode: "standard"` | Required + common fields | Default |
| `mode: "full"` | All properties + dependencies | Thorough pre-deploy check |
| `profile: "runtime"` | As if actually executing | Final check before activation |

### validate_workflow

Checks the entire workflow structure:
- Node connections are valid
- All referenced credentials exist
- Start node is present
- No circular dependencies
- Activation-required fields are set

---

## Common Errors and Fixes

### Required Field Missing

```
Error: "field 'channel' is required for operation 'post'"
Fix: Add the missing field for this resource/operation combination.
     Use get_node({nodeType, detail: "standard"}) to see what's required.
```

### displayOptions Mismatch

```
Error: "field 'sendBody' is not shown for current settings"
Meaning: You're setting a field that's hidden for your chosen method/operation.
Fix: Change the resource/operation to one where this field is visible,
     or remove the field from the config.
```

### Credential Not Found

```
Error: "credential 'My Slack API' not found"
Fix: Create the credential first:
     n8n_manage_credentials({action: "create", name: "My Slack API", type: "slackApi", ...})
     Then reference it by its returned ID.
```

### Invalid Connection

```
Error: "node 'NodeB' not found in workflow"
Fix: Ensure the target node exists before adding a connection to it.
     Use n8n_get_workflow to check the current node list.
```

### Empty Code Node

```
Error: "Code node has no code"
Fix: The jsCode/pythonCode field cannot be empty.
     Provide at least a minimal passthrough:
     return $input.all();
```

---

## False Positives — When to Ignore Validation Warnings

Some validation messages are informational warnings that don't block workflow execution:

| Warning | Safe to Ignore? | Context |
|---------|----------------|---------|
| `"no explicit pairedItem"` | Yes, often | Code nodes creating new items — add pairedItem if downstream Set nodes fail |
| `"node has no outgoing connections"` | Yes | Terminal nodes (Slack, email senders) intentionally have no output |
| `"deprecated property"` | Yes, usually | Old config that still works; update when convenient |
| `"credential type mismatch"` | No — fix it | Wrong credential type will fail at runtime |
| `"expression returns undefined"` | No — fix it | Data path is wrong |

**When in doubt:** run the workflow in test mode and observe the actual behavior rather than trusting warnings alone.

---

## Auto-Fix

`n8n_autofix_workflow` handles common fixable issues automatically:

```javascript
// Run auto-fix
n8n_autofix_workflow({ workflowId: "abc123" })

// What it fixes:
// - Missing pairedItem references
// - Incorrect return format in Code nodes
// - Empty optional fields that need defaults
// - Basic connection formatting issues

// What it does NOT fix:
// - Missing required fields (you must add them)
// - Wrong credential references
// - Logic errors in Code nodes
// - displayOptions mismatches
```

Always validate again after running auto-fix.

---

## Validation-Driven Development Pattern

The most reliable workflow configuration approach:

```
1. Scaffold node config with known required fields only
2. validate_node (minimal) → identify any missing required fields
3. Add missing fields
4. validate_node (runtime) → catch runtime-specific issues
5. Fix any runtime issues
6. Add node to workflow
7. validate_workflow → catch cross-node issues
8. Test with actual data in n8n UI
9. Activate
```

---

## Reference Files

- **`references/ERROR_CATALOG.md`** — Complete catalog of every validation error code, message, cause, and fix — organized by error type (required fields, displayOptions, credentials, connections, code)
- **`references/FALSE_POSITIVES.md`** — Detailed list of warnings that are safe to ignore vs. ones that will cause runtime failures, with context on when each applies
