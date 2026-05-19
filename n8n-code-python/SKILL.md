---
name: n8n-code-python
description: "Write Python code in n8n Code nodes. Use this skill when the user explicitly wants Python in an n8n Code node — for standard. Triggers: 'use n8n-code-python', 'n8n code python', 'n8n-code-python task."
allowed-tools: Glob, Grep, Read
---

# n8n Python Code Node (Beta)

Expert guidance for Python in n8n Code nodes.

---

## JavaScript First

Use **JavaScript for 95% of n8n use cases**. Choose Python only when:
- You need a specific Python standard library function (statistics, hashlib, etc.)
- The user explicitly prefers Python syntax
- The transformation maps cleanly to list comprehensions

**Why JavaScript is usually better:** full `$helpers.httpRequest()`, Luxon DateTime, better n8n docs, no library limitations.

---

## Quick Start

```python
# Standard template — "Run Once for All Items" mode
items = _input.all()

return [
    {
        "json": {
            **item["json"],
            "processed": True,
            "timestamp": datetime.now().isoformat()
        }
    }
    for item in items
]
```

**Three rules before writing any code:**
1. Return `[{"json": {...}}]` — list of dicts with `"json"` key → verify: step output matches expected outcome
2. Webhook data lives under `_json["body"]`, not `_json` directly → verify: step output matches expected outcome
3. No external libraries — standard library only → verify: step output matches expected outcome

---

## Mode Selection

| Mode | Data Access |
|------|-------------|
| **Run Once for All Items** (default, 95%) | `_input.all()` |
| **Run Once for Each Item** | `_input.item` |

---

## Python Modes

Use **Python (Beta)** — it gives access to `_input`, `_json`, `_node`, `_now`.

**Python (Native) Beta** only provides `_items`/`_item` — more limited, use only if needed.

---

## Data Access

```python
# All items
all_items = _input.all()

# First item
first = _input.first()
data = first["json"]

# Each Item mode only
current = _input.item

# Reference another node
webhook_data = _node["Webhook"].first()["json"]  # ✅ correct

# Webhook data is nested under ["body"]
name = _json["body"]["name"]                     # ✅ correct
name = _json["name"]                             # ❌ KeyError!

# Always use .get() for safe access
email = _json.get("body", {}).get("email", "")
```

---

## Return Format

```python
# ✅ Single result
return [{"json": {"field": value}}]

# ✅ List comprehension (Pythonic)
return [{"json": {"id": item["json"]["id"]}} for item in _input.all()]

# ✅ Empty result
return []

# ❌ Dict without list wrapper
return {"json": {"field": value}}

# ❌ List without "json" key
return [{"field": value}]
```

---

## Critical: No External Libraries

```python
# ❌ NOT AVAILABLE — will raise ModuleNotFoundError
import requests
import pandas
import numpy
import scipy
from bs4 import BeautifulSoup

# ✅ AVAILABLE — standard library only
import json, re, base64, hashlib, math, random, statistics
from datetime import datetime, timedelta
import urllib.parse
```

**Workarounds:**
- Need HTTP? → Use HTTP Request node before Code node, or switch to JavaScript
- Need pandas/numpy? → Use `statistics` module, or switch to JavaScript
- Need web scraping? → Use HTTP Request + HTML Extract nodes

---

## Top 5 Errors

**#1 Importing external libraries** — `ModuleNotFoundError: No module named 'requests'`
→ Use HTTP Request node or switch to JavaScript

**#2 No return statement** — always `return` something

**#3 Wrong return format** — must be `[{"json": {}}]` not `{"json": {}}`

**#4 KeyError on direct dict access:**
```python
# ❌ Crashes if field missing
name = _json["user"]["name"]
# ✅ Safe
name = _json.get("user", {}).get("name", "Unknown")
```

**#5 Webhook nesting** — data is under `["body"]`, always

---

## Standard Library Quick Reference

```python
import json
data = json.loads(json_string)
out = json.dumps({"key": "val"})

from datetime import datetime, timedelta
now = datetime.now()
tomorrow = now + timedelta(days=1)
formatted = now.strftime("%Y-%m-%d")

import re
matches = re.findall(r'\d+', text)
cleaned = re.sub(r'[^\w\s]', '', text)

import base64
encoded = base64.b64encode(data.encode()).decode()

import hashlib
hash_val = hashlib.sha256(text.encode()).hexdigest()

import urllib.parse
params = urllib.parse.urlencode({"key": "value"})

from statistics import mean, median, stdev
avg = mean([1, 2, 3, 4, 5])
```

---

## Common Patterns

```python
# Filtering
valid = [item for item in _input.all() if item["json"].get("active")]

# Aggregation
total = sum(item["json"].get("amount", 0) for item in _input.all())

# Transformation
return [
    {"json": {**item["json"], "upper_name": item["json"].get("name","").upper()}}
    for item in _input.all()
]

# Regex extraction
import re
pattern = r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b'
emails = list(set(
    email
    for item in _input.all()
    for email in re.findall(pattern, item["json"].get("text",""))
))
return [{"json": {"emails": emails, "count": len(emails)}}]

# Statistics
from statistics import mean, median, stdev
values = [item["json"].get("value",0) for item in _input.all() if "value" in item["json"]]
return [{"json": {"mean": mean(values), "median": median(values), "count": len(values)}}]
```

---

## Production Gotchas

**Node reference syntax:**
```python
# ❌ Wrong
data = _node['HTTP Request']['json']
# ✅ Correct
data = _node['HTTP Request'].first()['json']
```

**SplitInBatches:** `main[0]` = done, `main[1]` = loop body (same as JavaScript).

**Cross-iteration accumulation:** `$getWorkflowStaticData('global')` may not be available in Python Beta. Use a JavaScript Code node for the accumulator step instead.

---

## Reference Files

- **`references/DATA_ACCESS.md`** — Full Python data access patterns, _input/_json/_node/_items syntax
- **`references/COMMON_PATTERNS.md`** — 10 Python-specific production patterns
- **`references/ERROR_PATTERNS.md`** — Error messages and fixes specific to Python Code nodes
- **`references/STANDARD_LIBRARY.md`** — Complete standard library reference for n8n Python

## When NOT to use

- Task is unrelated to n8n code python — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | N8N Code Python needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for n8n code python
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving n8n code python
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
