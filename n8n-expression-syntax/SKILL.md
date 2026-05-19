---
name: n8n-expression-syntax
description: 'Write and fix n8n expression syntax. Trigger: configuring any n8n node field that references data from. Triggers: "use n8n-expression-syntax", "automate n8n expression syntax", "n8n expression syntax.'
allowed-tools: Glob, Grep, Read
---

# n8n Expression Syntax

Expert guide for writing correct n8n expressions.

---

## Expression Format

All dynamic content uses **double curly braces**: `{{expression}}`

```
✅ {{$json.email}}
✅ {{$json.body.name}}
✅ {{$node["HTTP Request"].json.data}}
❌ $json.email       (no braces — treated as literal text)
❌ {$json.email}     (single braces — invalid)
❌ {{ $json.email }} (spaces inside braces — use none or consistent)
```

---

## Core Variables

| Variable | Purpose | Example |
|----------|---------|---------|
| `$json` | Current node's output data | `{{$json.email}}` |
| `$node["Name"]` | Any previous node's data | `{{$node["HTTP Request"].json.status}}` |
| `$now` | Current datetime (Luxon) | `{{$now.toFormat('yyyy-MM-dd')}}` |
| `$today` | Today at midnight | `{{$today.toISO()}}` |
| `$env` | Environment variables | `{{$env.API_KEY}}` |
| `$workflow` | Workflow metadata | `{{$workflow.name}}` |
| `$execution` | Execution metadata | `{{$execution.id}}` |
| `$itemIndex` | Current item index (0-based) | `{{$itemIndex}}` |
| `$prevNode` | Previous node name/index | `{{$prevNode.name}}` |

---

## Webhook Data — Most Common Mistake

Webhook data is **always nested under `.body`**:

```javascript
// Webhook sends: { "name": "John", "email": "john@example.com" }

// n8n wraps it as:
{
  "headers": { ... },
  "params":  { ... },
  "query":   { ... },
  "body": {              // ← your data is HERE
    "name": "John",
    "email": "john@example.com"
  }
}

// ❌ WRONG — returns undefined
{{$json.name}}
{{$json.email}}

// ✅ CORRECT
{{$json.body.name}}
{{$json.body.email}}
```

---

## Access Patterns

```javascript
// Nested fields
{{$json.user.email}}
{{$json.data[0].name}}
{{$json.items[0].id}}

// Fields with spaces (bracket notation)
{{$json['field name']}}
{{$json['user data']['first name']}}

// Previous node (name must match exactly, case-sensitive)
{{$node["Set"].json.value}}
{{$node["HTTP Request"].json.data.id}}
{{$node["Webhook"].json.body.email}}

// Date math
{{$now.plus({days: 7}).toFormat('yyyy-MM-dd')}}
{{$now.minus({hours: 1}).toISO()}}

// Conditional (ternary)
{{$json.status === 'active' ? 'Yes' : 'No'}}

// String template
{{"Hello " + $json.body.name + "!"}}

// Null coalescing
{{$json.value ?? 'default'}}
{{$json.user?.email ?? 'no-email'}}
```

---

## Where NOT to Use Expressions

**Code Nodes** — use JavaScript directly, no `{{ }}`:
```javascript
// ❌ Wrong inside Code node
const email = '={{ $json.email }}';

// ✅ Correct inside Code node
const email = $input.first().json.email;
const email = $json.email;
```

**Credential fields** — credentials don't support expressions.

**Static webhook paths** — paths must be static strings.

---

## $env Caveat

Some n8n instances set `N8N_BLOCK_ENV_ACCESS_IN_NODE=true`, which blocks `$env` entirely. If `{{$env.KEY}}` returns an error, use:
- Credentials (recommended)
- Set node with manually entered values
- Webhook query parameters

---

## Common Mistakes Checklist

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Expression returns `undefined` | Wrong path | Check exact field names with a Debug Helper node |
| Webhook data missing | Not using `.body` | Change `$json.field` → `$json.body.field` |
| "Node not found" error | Wrong node name | Match exact name (case-sensitive, with spaces) |
| Expression treated as literal text | Missing `{{ }}` | Wrap in double curly braces |
| Crashes on null | No optional chaining | Use `?.` and `?? 'fallback'` |

---

## Reference Files

- **`references/EXAMPLES.md`** — 30+ real-world expression examples by use case (Slack, email, HTTP, webhooks, dates, conditionals)
- **`references/COMMON_MISTAKES.md`** — Detailed error messages, exact causes, and fixes for expression failures

---

## Quick Debug Technique

Add a **Set** node → use expression → check the preview panel. The preview shows exactly what the expression evaluates to before you wire it to the next node.

## When NOT to use

- Task is unrelated to n8n expression syntax — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | N8N Expression Syntax needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for n8n expression syntax
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving n8n expression syntax
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
