# Mode: optimize — Performance Optimization

Asks Codex to identify and fix performance bottlenecks in a target module.

## Usage

```
/codex-optimize <target> [context]
```

Examples:
- `/codex-optimize projects/media_pipeline/pipeline.py`
- `/codex-optimize agents/media_fetcher.py parallel media downloads`
- `/codex-optimize src/api/jobs.py N+1 query issue`

## Execution Steps

### Step 1 — Profile First (if possible)

```bash
python -m cProfile -s cumulative <script> 2>&1 | head -30
```

Or check for known bottlenecks in the target file.

### Step 2 — Run Codex

```bash
codex --approval-policy suggest "
Analyze [TARGET] for performance bottlenecks and propose optimizations.

FOCUS AREAS:
1. ASYNC/CONCURRENCY — blocking I/O in async code, missed parallelism opportunities
   - Sequential awaits that could be asyncio.gather()
   - Synchronous file/network calls in async functions
   - Missing async for LLM calls that could run in parallel

2. DATABASE — N+1 queries, missing indexes, over-fetching
   - Supabase queries inside loops
   - Selecting all columns when only a few are needed
   - Missing .limit() on unbounded queries

3. LLM CALLS — unnecessary sequential calls
   - Multiple call_llm() calls that don't depend on each other
   - Large prompts that could be trimmed without losing quality
   - Retry logic with no backoff

4. MEMORY — large objects held in memory longer than needed
   - Loading entire files into memory instead of streaming
   - Accumulating results in lists that could be processed incrementally

5. REDUNDANT WORK — recomputing the same value multiple times
   - Functions called in loops that return the same value each iteration
   - Missing caching for expensive pure computations

For each bottleneck:
1. File + line number
2. Severity: HIGH | MEDIUM | LOW (based on call frequency × cost)
3. Description of the bottleneck
4. Concrete optimized version of the code
5. Expected improvement

Only propose changes that have clear, measurable benefit. Do not optimize for hypothetical scale.
"
```

### Step 3 — Apply Selected Optimizations

For each HIGH severity optimization the user approves, use `/codex-fix` to implement it.
