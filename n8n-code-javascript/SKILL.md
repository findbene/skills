---
name: n8n-code-javascript
description: Write JavaScript code in n8n Code nodes. Always use this skill when any n8n workflow needs a Code node — for data aggregation, filtering, API calls, format conversion, batch processing, custom transformations, SplitInBatches loops, or any custom JavaScript logic. Use whenever you see $input/$json/$node syntax, $helpers.httpRequest, DateTime/Luxon usage, pairedItem issues, or cross-iteration data problems. Don't try to write n8n Code node JavaScript from memory — consult this skill first to avoid the most common production mistakes.
---

# n8n JavaScript Code Node

Expert guidance for writing JavaScript in n8n Code nodes.

---

## Quick Start

```javascript
// Standard template — "Run Once for All Items" mode (use this 95% of the time)
const items = $input.all();

return items.map(item => ({
  json: {
    ...item.json,
    processed: true,
    timestamp: new Date().toISOString()
  }
}));
```

**Three rules before writing any code:**
1. Always return `[{json: {...}}]` — array of objects with `json` key
2. Webhook data lives under `$json.body`, not `$json` directly
3. Use `$input.all()` for "All Items" mode, `$input.item` for "Each Item" mode

---

## Mode Selection

| Mode | When to Use | Data Access |
|------|-------------|-------------|
| **Run Once for All Items** (default) | 95% of cases — aggregation, filtering, batch ops | `$input.all()` |
| **Run Once for Each Item** | Each item needs independent processing | `$input.item` |

When unsure → use "All Items" mode. You can always loop inside.

---

## Data Access Patterns

```javascript
// Most common: process all items
const all = $input.all();

// Single item / API responses
const first = $input.first();
const data = first.json;

// Reference another node by name
const webhookData = $('Webhook').first().json;   // ✅ correct
const bad = $('HTTP Request').json;              // ❌ wrong — need .first()

// Each Item mode only
const current = $input.item;
```

**Webhook data is always nested:**
```javascript
// ❌ WRONG — returns undefined
const email = $json.email;

// ✅ CORRECT
const email = $json.body.email;
const name = $input.first().json.body.name;
```

---

## Return Format

```javascript
// ✅ Single result
return [{ json: { field: value } }];

// ✅ Multiple results
return items.map(item => ({ json: { id: item.json.id } }));

// ✅ Empty (filter everything out)
return [];

// ❌ Missing array wrapper
return { json: { field: value } };

// ❌ Missing json key
return [{ field: value }];
```

---

## Top 5 Errors to Avoid

**#1 No return statement** — always `return` something

**#2 Using n8n expression syntax in code:**
```javascript
// ❌ Wrong — expressions don't work inside Code nodes
const val = "{{ $json.field }}";
// ✅ Correct
const val = $input.first().json.field;
```

**#3 Wrong return wrapper** — must be `[{json: {}}]` not `{json: {}}`

**#4 Missing null checks:**
```javascript
// ❌ Crashes if user is null
const email = item.json.user.email;
// ✅ Safe
const email = item.json?.user?.email ?? 'fallback@example.com';
```

**#5 Webhook nesting** — data is under `.body`, always

---

## Built-in Functions

```javascript
// HTTP requests
const response = await $helpers.httpRequest({
  method: 'POST',
  url: 'https://api.example.com/data',
  headers: { 'Authorization': 'Bearer token' },
  body: { key: 'value' }
});

// Date/time (Luxon)
const now = DateTime.now();
const formatted = now.toFormat('yyyy-MM-dd');
const tomorrow = now.plus({ days: 1 });

// JSON path queries
const adults = $jmespath(data, 'users[?age >= `18`]');
```

---

## Production Gotchas (Hard-Won)

### SplitInBatches Outputs Are Backwards
- `main[0]` = **done** (fires once after all batches)
- `main[1]` = **loop body** (fires each batch)

Add a **Limit 1** node after the done output.

### Cross-Iteration Data Accumulation
`$('NodeInsideLoop').all()` only returns the **last batch** — not all batches combined. Use workflow static data to accumulate:

```javascript
// Before loop — reset
const staticData = $getWorkflowStaticData('global');
staticData.results = [];
return $input.all();

// Inside loop — accumulate
const staticData = $getWorkflowStaticData('global');
for (const item of $input.all()) {
  staticData.results.push(item.json);
}
return $input.all().map(i => ({ json: i.json }));

// After loop — read all
const staticData = $getWorkflowStaticData('global');
const all = staticData.results || [];
```

### pairedItem for New Output Items
When creating items that don't map 1:1 to inputs, include `pairedItem` to avoid downstream Set node errors:

```javascript
return $input.all().map((item, i) => ({
  json: { newField: 'value' },
  pairedItem: { item: i }
}));
```

### Float Comparison for Prices
```javascript
// ❌ Floating point noise
if (newPrice !== oldPrice) { ... }
// ✅ Compare at cent level
if (Math.round(newPrice * 100) !== Math.round(oldPrice * 100)) { ... }
```

---

## When to Use Code Node vs Other Nodes

| Task | Use This Instead |
|------|-----------------|
| Simple field mapping | Set node |
| Basic filtering | Filter node |
| Simple if/else | IF or Switch node |
| HTTP request only | HTTP Request node |
| **Complex multi-step logic** | **Code node ✅** |

---

## Reference Files

Read these when you need deeper detail:

- **`references/DATA_ACCESS.md`** — Complete guide to `$input`, `$json`, `$node`, `$('name')`, webhook structures, binary data
- **`references/COMMON_PATTERNS.md`** — 10 production-tested patterns: aggregation, deduplication, ranked filtering, multi-source merging, etc.
- **`references/ERROR_PATTERNS.md`** — Detailed error messages, causes, and fixes for the most common Code node failures
- **`references/BUILTIN_FUNCTIONS.md`** — Full reference for `$helpers`, `DateTime`/Luxon, `$jmespath`, `$getWorkflowStaticData`

---

## Pre-Deploy Checklist

- [ ] Return statement exists and returns `[{json: {...}}]`
- [ ] Webhook data accessed via `.body`
- [ ] No `{{ }}` expressions inside code
- [ ] Null checks on potentially missing fields
- [ ] Mode is "All Items" unless you specifically need per-item isolation
- [ ] `pairedItem` included if creating new items
