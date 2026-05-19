---
name: langsmith-fetch
description: 'Debug LangChain and LangGraph agents by fetching execution traces from LangSmith Studio. Triggers: "use langsmith-fetch", "langsmith fetch", "langsmith task".'
allowed-tools: Bash, Glob, Grep, Read
---

# LangSmith Fetch - Agent Debugging Skill

Debug LangChain and LangGraph agents by fetching execution traces directly from LangSmith Studio in your terminal.

## When to Use This Skill

Automatically activate when user mentions:
- 🐛 "Debug my agent" or "What went wrong?"
- 🔍 "Show me recent traces" or "What happened?"
- ❌ "Check for errors" or "Why did it fail?"
- 💾 "Analyze memory operations" or "Check LTM"
- 📊 "Review agent performance" or "Check token usage"
- 🔧 "What tools were called?" or "Show execution flow"

## Prerequisites

### 1. Install langsmith-fetch
```bash
pip install langsmith-fetch
```

### 2. Set Environment Variables
```bash
export LANGSMITH_API_KEY="your_langsmith_api_key"
export LANGSMITH_PROJECT="your_project_name"
```

**Verify setup:**
```bash
echo $LANGSMITH_API_KEY
echo $LANGSMITH_PROJECT
```

## Core Workflows

### Workflow 1: Quick Debug Recent Activity

**When user asks:** "What just happened?" or "Debug my agent"

**Execute:**
```bash
langsmith-fetch traces --last-n-minutes 5 --limit 5 --format pretty
```

**Analyze and report:**
1. ✅ Number of traces found → verify: step output matches expected outcome
2. ⚠️ Any errors or failures → verify: step output matches expected outcome
3. 🛠️ Tools that were called → verify: step output matches expected outcome
4. ⏱️ Execution times → verify: step output matches expected outcome
5. 💰 Token usage → verify: step output matches expected outcome

**Example response format:**
```
Found 3 traces in the last 5 minutes:

Trace 1: ✅ Success
- Agent: memento
- Tools: recall_memories, create_entities
- Duration: 2.3s
- Tokens: 1,245

Trace 2: ❌ Error
- Agent: cypher
- Error: "Neo4j connection timeout"
- Duration: 15.1s
- Failed at: search_nodes tool

Trace 3: ✅ Success
- Agent: memento
- Tools: store_memory
- Duration: 1.8s
- Tokens: 892

💡 Issue found: Trace 2 failed due to Neo4j timeout. Recommend checking database connection.
```

---

### Workflow 2: Deep Dive Specific Trace

**When user provides:** Trace ID or says "investigate that error"

**Execute:**
```bash
langsmith-fetch trace <trace-id> --format json
```

**Analyze JSON and report:**
1. 🎯 What the agent was trying to do → verify: step output matches expected outcome
2. 🛠️ Which tools were called (in order) → verify: step output matches expected outcome
3. ✅ Tool results (success/failure) → verify: step output matches expected outcome
4. ❌ Error messages (if any) → verify: step output matches expected outcome
5. 💡 Root cause analysis → verify: step output matches expected outcome
6. 🔧 Suggested fix → verify: diff matches intended change

**Example response format:**
```
Deep Dive Analysis - Trace abc123

Goal: User asked "Find all projects in Neo4j"

Execution Flow:
1. ✅ search_nodes(query: "projects") → verify: step output matches expected outcome
   → Found 24 nodes

2. ❌ get_node_details(node_id: "proj_123") → verify: step output matches expected outcome
   → Error: "Node not found"
   → This is the failure point

3. ⏹️ Execution stopped → verify: step output matches expected outcome

Root Cause:
The search_nodes tool returned node IDs that no longer exist in the database,
possibly due to recent deletions.

Suggested Fix:
1. Add error handling in get_node_details tool → verify: package installed + import succeeds
2. Filter deleted nodes in search results → verify: step output matches expected outcome
3. Update cache invalidation strategy → verify: step output matches expected outcome

Token Usage: 1,842 tokens ($0.0276)
Execution Time: 8.7 seconds
```

---

### Workflow 3: Export Debug Session

**When user says:** "Save this session" or "Export traces"

**Execute:**
```bash
# Create session folder with timestamp
SESSION_DIR="langsmith-debug/session-$(date +%Y%m%d-%H%M%S)"
mkdir -p "$SESSION_DIR"

# Export traces
langsmith-fetch traces "$SESSION_DIR/traces" --last-n-minutes 30 --limit 50 --include-metadata

# Export threads (conversations)
langsmith-fetch threads "$SESSION_DIR/threads" --limit 20
```

**Report:**
```
✅ Session exported successfully!

Location: langsmith-debug/session-20251224-143022/
- Traces: 42 files
- Threads: 8 files

You can now:
1. Review individual trace files → verify: step output matches expected outcome
2. Share folder with team → verify: step output matches expected outcome
3. Analyze with external tools → verify: step output matches expected outcome
4. Archive for future reference → verify: step output matches expected outcome

Session size: 2.3 MB
```

---

### Workflow 4: Error Detection

**When user asks:** "Show me errors" or "What's failing?"

**Execute:**
```bash
# Fetch recent traces
langsmith-fetch traces --last-n-minutes 30 --limit 50 --format json > recent-traces.json

# Search for errors
grep -i "error\|failed\|exception" recent-traces.json
```

**Analyze and report:**
1. 📊 Total errors found → verify: step output matches expected outcome
2. ❌ Error types and frequency → verify: step output matches expected outcome
3. 🕐 When errors occurred → verify: step output matches expected outcome
4. 🎯 Which agents/tools failed → verify: step output matches expected outcome
5. 💡 Common patterns → verify: step output matches expected outcome

**Example response format:**
```
Error Analysis - Last 30 Minutes

Total Traces: 50
Failed Traces: 7 (14% failure rate)

Error Breakdown:
1. Neo4j Connection Timeout (4 occurrences) → verify: step output matches expected outcome
   - Agent: cypher
   - Tool: search_nodes
   - First occurred: 14:32
   - Last occurred: 14:45
   - Pattern: Happens during peak load

2. Memory Store Failed (2 occurrences) → verify: step output matches expected outcome
   - Agent: memento
   - Tool: store_memory
   - Error: "Pinecone rate limit exceeded"
   - Occurred: 14:38, 14:41

3. Tool Not Found (1 occurrence) → verify: step output matches expected outcome
   - Agent: sqlcrm
   - Attempted tool: "export_report" (doesn't exist)
   - Occurred: 14:35

💡 Recommendations:
1. Add retry logic for Neo4j timeouts → verify: package installed + import succeeds
2. Implement rate limiting for Pinecone → verify: step output matches expected outcome
3. Fix sqlcrm tool configuration → verify: diff matches intended change
```

---

## Common Use Cases

### Use Case 1: "Agent Not Responding"

**User says:** "My agent isn't doing anything"

**Steps:**
1. Check if traces exist: → verify: all checks pass
   ```bash
   langsmith-fetch traces --last-n-minutes 5 --limit 5
   ```

2. **If NO traces found:**
   - Tracing might be disabled
   - Check: `LANGCHAIN_TRACING_V2=true` in environment
   - Check: `LANGCHAIN_API_KEY` is set
   - Verify agent actually ran

3. **If traces found:**
   - Review for errors
   - Check execution time (hanging?)
   - Verify tool calls completed

---

### Use Case 2: "Wrong Tool Called"

**User says:** "Why did it use the wrong tool?"

**Steps:**
1. Get the specific trace → verify: step output matches expected outcome
2. Review available tools at execution time → verify: step output matches expected outcome
3. Check agent's reasoning for tool selection → verify: all checks pass
4. Examine tool descriptions/instructions → verify: step output matches expected outcome
5. Suggest prompt or tool config improvements → verify: step output matches expected outcome

---

### Use Case 3: "Memory Not Working"

**User says:** "Agent doesn't remember things"

**Steps:**
1. Search for memory operations: → verify: step output matches expected outcome
   ```bash
   langsmith-fetch traces --last-n-minutes 10 --limit 20 --format raw | grep -i "memory\|recall\|store"
   ```

2. Check:
   - Were memory tools called?
   - Did recall return results?
   - Were memories actually stored?
   - Are retrieved memories being used?

---

### Use Case 4: "Performance Issues"

**User says:** "Agent is too slow"

**Steps:**
1. Export with metadata: → verify: step output matches expected outcome
   ```bash
   langsmith-fetch traces ./perf-analysis --last-n-minutes 30 --limit 50 --include-metadata
   ```

2. Analyze:
   - Execution time per trace
   - Tool call latencies
   - Token usage (context size)
   - Number of iterations
   - Slowest operations

3. Identify bottlenecks and suggest optimizations → verify: step output matches expected outcome

---

## Output Format Guide

### Pretty Format (Default)
```bash
langsmith-fetch traces --limit 5 --format pretty
```
**Use for:** Quick visual inspection, showing to users

### JSON Format
```bash
langsmith-fetch traces --limit 5 --format json
```
**Use for:** Detailed analysis, syntax-highlighted review

### Raw Format
```bash
langsmith-fetch traces --limit 5 --format raw
```
**Use for:** Piping to other commands, automation

---

## Advanced Features

### Time-Based Filtering
```bash
# After specific timestamp
langsmith-fetch traces --after "2025-12-24T13:00:00Z" --limit 20

# Last N minutes (most common)
langsmith-fetch traces --last-n-minutes 60 --limit 100
```

### Include Metadata
```bash
# Get extra context
langsmith-fetch traces --limit 10 --include-metadata

# Metadata includes: agent type, model, tags, environment
```

### Concurrent Fetching (Faster)
```bash
# Speed up large exports
langsmith-fetch traces ./output --limit 100 --concurrent 10
```

---

## Troubleshooting

### "No traces found matching criteria"

**Possible causes:**
1. No agent activity in the timeframe → verify: step output matches expected outcome
2. Tracing is disabled → verify: step output matches expected outcome
3. Wrong project name → verify: step output matches expected outcome
4. API key issues → verify: step output matches expected outcome

**Solutions:**
```bash
# 1. Try longer timeframe
langsmith-fetch traces --last-n-minutes 1440 --limit 50

# 2. Check environment
echo $LANGSMITH_API_KEY
echo $LANGSMITH_PROJECT

# 3. Try fetching threads instead
langsmith-fetch threads --limit 10

# 4. Verify tracing is enabled in your code
# Check for: LANGCHAIN_TRACING_V2=true
```

### "Project not found"

**Solution:**
```bash
# View current config
langsmith-fetch config show

# Set correct project
export LANGSMITH_PROJECT="correct-project-name"

# Or configure permanently
langsmith-fetch config set project "your-project-name"
```

### Environment variables not persisting

**Solution:**
```bash
# Add to shell config file (~/.bashrc or ~/.zshrc)
echo 'export LANGSMITH_API_KEY="your_key"' >> ~/.bashrc
echo 'export LANGSMITH_PROJECT="your_project"' >> ~/.bashrc

# Reload shell config
source ~/.bashrc
```

---

## Best Practices

### 1. Regular Health Checks
```bash
# Quick check after making changes
langsmith-fetch traces --last-n-minutes 5 --limit 5
```

### 2. Organized Storage
```
langsmith-debug/
├── sessions/
│   ├── 2025-12-24/
│   └── 2025-12-25/
├── error-cases/
└── performance-tests/
```

### 3. Document Findings
When you find bugs:
1. Export the problematic trace → verify: step output matches expected outcome
2. Save to `error-cases/` folder → verify: step output matches expected outcome
3. Note what went wrong in a README → verify: file content matches expected shape
4. Share trace ID with team → verify: step output matches expected outcome

### 4. Integration with Development
```bash
# Before committing code
langsmith-fetch traces --last-n-minutes 10 --limit 5

# If errors found
langsmith-fetch trace <error-id> --format json > pre-commit-error.json
```

---

## Quick Reference

```bash
# Most common commands

# Quick debug
langsmith-fetch traces --last-n-minutes 5 --limit 5 --format pretty

# Specific trace
langsmith-fetch trace <trace-id> --format pretty

# Export session
langsmith-fetch traces ./debug-session --last-n-minutes 30 --limit 50

# Find errors
langsmith-fetch traces --last-n-minutes 30 --limit 50 --format raw | grep -i error

# With metadata
langsmith-fetch traces --limit 10 --include-metadata
```

---

## Resources

- **LangSmith Fetch CLI:** https://github.com/langchain-ai/langsmith-fetch
- **LangSmith Studio:** https://smith.langchain.com/
- **LangChain Docs:** https://docs.langchain.com/
- **This Skill Repo:** https://github.com/OthmanAdi/langsmith-fetch-skill

---

## Notes for Claude

- Always check if `langsmith-fetch` is installed before running commands
- Verify environment variables are set
- Use `--format pretty` for human-readable output
- Use `--format json` when you need to parse and analyze data
- When exporting sessions, create organized folder structures
- Always provide clear analysis and actionable insights
- If commands fail, help troubleshoot configuration issues

---

**Version:** 0.1.0
**Author:** Ahmad Othman Ammar Adi
**License:** MIT
**Repository:** https://github.com/OthmanAdi/langsmith-fetch-skill

## When NOT to use

- Task is unrelated to langsmith fetch — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Langsmith Fetch needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for langsmith fetch
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow


## References

See `references/details.md` for extended sections.

## Examples

### Example 1 — Standard case
- Input: User invokes this skill for the typical use case
- Action: Follow the numbered process above end-to-end
- Output: Result matching the Output Contract

### Example 2 — Edge case
- Input: Unusual or boundary input matching the When-NOT triggers
- Action: Either route to the right skill or apply the documented fallback
- Output: Either correct hand-off or graceful no-op
