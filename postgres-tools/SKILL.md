---
name: postgres-tools
description: 'PostgreSQL database query and management via MCP for executing SQL, inspecting schemas, and running database operations. Triggers: "use postgres-tools", "postgres tools", "postgres task".'
allowed-tools: Glob, Grep, Read
---

# PostgreSQL Tools

Direct PostgreSQL database interaction via MCP server.

## Setup

The MCP server connects to a PostgreSQL instance via connection URL. Default: `postgresql://localhost:5432/postgres`. To change the target database, update the connection URL in `~/.claude/settings.json` under `mcpServers.postgres.args`.

## Available Tools

| Tool | Purpose |
|------|---------|
| `query` | Execute a read-only SQL query and return results |

The server exposes each table as a resource with its schema (columns, types, constraints).

## Common Workflows

### Inspect Database Schema
```
query("SELECT table_name FROM information_schema.tables WHERE table_schema = 'public'")
```

### View Table Structure
```
query("SELECT column_name, data_type, is_nullable, column_default FROM information_schema.columns WHERE table_name = 'users'")
```

### Query Data
```
query("SELECT * FROM users LIMIT 10")
```

### Check Indexes
```
query("SELECT indexname, indexdef FROM pg_indexes WHERE tablename = 'users'")
```

### List Foreign Keys
```
query("""
  SELECT tc.constraint_name, tc.table_name, kcu.column_name,
         ccu.table_name AS foreign_table_name, ccu.column_name AS foreign_column_name
  FROM information_schema.table_constraints AS tc
  JOIN information_schema.key_column_usage AS kcu ON tc.constraint_name = kcu.constraint_name
  JOIN information_schema.constraint_column_usage AS ccu ON ccu.constraint_name = tc.constraint_name
  WHERE tc.constraint_type = 'FOREIGN KEY'
""")
```

### Check Active Connections
```
query("SELECT datname, usename, state, query FROM pg_stat_activity WHERE state = 'active'")
```

## Tips

- The server runs queries as **read-only** by default for safety
- Connection URL format: `postgresql://user:password@host:port/database`
- To target a different database, update the args in `~/.claude/settings.json`
- Use `information_schema` views for portable schema introspection
- Use `pg_catalog` tables for PostgreSQL-specific metadata

## When NOT to use

- Task is unrelated to postgres tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Postgres Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for postgres tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving postgres tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
