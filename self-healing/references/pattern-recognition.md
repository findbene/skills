# Error Pattern Recognition

## Failure Pattern Database (Supabase)
```sql
CREATE TABLE error_patterns (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    error_type TEXT NOT NULL,
    error_message TEXT,
    agent_name TEXT,
    occurrence_count INTEGER DEFAULT 1,
    first_seen TIMESTAMPTZ DEFAULT now(),
    last_seen TIMESTAMPTZ DEFAULT now(),
    resolution_applied TEXT,
    resolved BOOLEAN DEFAULT false,
    created_at TIMESTAMPTZ DEFAULT now()
);

-- Find unresolved recurring patterns
SELECT error_type, error_message, agent_name, occurrence_count
FROM error_patterns
WHERE resolved = false AND occurrence_count >= 3
ORDER BY occurrence_count DESC;
```

## Pattern Analyzer
```python
from collections import Counter

def analyze_error_patterns(error_log: list[dict]) -> dict:
    error_types = Counter(e.get("error_type") for e in error_log)
    recurring = {t: c for t, c in error_types.items() if c >= 3}

    return {
        "recurring_errors": recurring,
        "most_common": error_types.most_common(5),
        "recommendation": generate_fix(recurring)
    }
```

## Common Agent Failure Patterns

### Pattern 1: Prompt Output Format Drift
**Symptom**: Agent produces correct output at first, then drifts to wrong format
**Fix**:
- Reduce temperature (0.1–0.3 for format-sensitive tasks)
- Add output format reminder at END of prompt (not just beginning)
- Add schema assertion after generation

```python
def validate_script_output(output: str) -> bool:
    word_count = len(output.split())
    if word_count < 50 or word_count > 200:
        return False
    has_hook = len(output.split('\n')[0]) > 10  # first line not empty
    return has_hook
```

### Pattern 2: API Rate Limit Cascade
**Symptom**: One 429 causes all downstream tasks to fail
**Fix**: Semaphore at API layer

```python
import asyncio

CLAUDE_SEM = asyncio.Semaphore(5)    # max 5 concurrent
ELEVENLABS_SEM = asyncio.Semaphore(2)  # max 2 concurrent

async def safe_generate_script(niche: str) -> str:
    async with CLAUDE_SEM:
        return await generate_script_async(niche)
```

### Pattern 3: Context Memory Exhaustion
**Symptom**: Agent forgets earlier decisions, outputs become generic
**Fix**: Compress old context, extract to external memory

```python
def compress_history(messages: list[dict], target_tokens: int = 4000) -> list[dict]:
    if not exceeds_budget(messages, target_tokens):
        return messages
    recent = messages[-5:]
    older = messages[1:-5]
    summary = summarize_messages(older)
    return [
        messages[0],  # system prompt
        {"role": "assistant", "content": f"[Summary: {summary}]"},
        *recent
    ]
```

### Pattern 4: Silent Failure (No Error Raised)
**Symptom**: Task appears to complete but output is wrong/empty
**Fix**: Always validate outputs; don't trust 200 status alone

```python
def validate_api_response(response: dict, required_fields: list[str]) -> None:
    for field in required_fields:
        if field not in response:
            raise ValueError(f"Missing '{field}' in response: {response}")
    if response.get("error") or response.get("status") == "failed":
        raise ValueError(f"API returned error in 200: {response}")
```

### Pattern 5: Retry Storm
**Symptom**: Retries amplify load, causing cascading failures across all workers
**Fix**: Jitter + exponential backoff + circuit breaker

```python
import random
import time

def retry_with_jitter(func, max_attempts=3, base_delay=2.0):
    for attempt in range(1, max_attempts + 1):
        try:
            return func()
        except Exception as e:
            if attempt == max_attempts:
                raise
            # Jitter prevents thundering herd
            delay = base_delay * (2 ** (attempt - 1)) * (0.5 + random.random())
            time.sleep(min(delay, 60))
```
