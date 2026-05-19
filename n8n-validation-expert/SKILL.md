---
name: n8n-validation-expert
description: "Validate and auto-fix n8n workflow configurations. Trigger: running validate_node or validate_workflow, interpreting validation errors, deciding whether to fix or ignore a validation message, working."
allowed-tools: Glob, Grep, Read
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
2. [Fix issues] → verify: diff matches intended change
3. validate_node({nodeType, config, profile: "runtime"}) → full runtime check
4. n8n_create_workflow or n8n_update_partial_workflow → verify: output exists + parses without error
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
1. Scaffold node config with known required fields only → verify: step output matches expected outcome
2. validate_node (minimal) → identify any missing required fields
3. Add missing fields → verify: dependency resolves + import works
4. validate_node (runtime) → catch runtime-specific issues
5. Fix any runtime issues → verify: command exit code 0
6. Add node to workflow → verify: dependency resolves + import works
7. validate_workflow → catch cross-node issues
8. Test with actual data in n8n UI → verify: all checks pass
9. Activate
```

---

## Reference Files

- **`references/ERROR_CATALOG.md`** — Complete catalog of every validation error code, message, cause, and fix — organized by error type (required fields, displayOptions, credentials, connections, code)
- **`references/FALSE_POSITIVES.md`** — Detailed list of warnings that are safe to ignore vs. ones that will cause runtime failures, with context on when each applies

## When NOT to use

- Task is unrelated to n8n validation expert — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | N8N Validation Expert needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for n8n validation expert
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving n8n validation expert
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
