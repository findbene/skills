---
name: postgres-tools
description: "PostgreSQL database query and management via MCP for executing SQL, inspecting schemas, and running database operations. Use this skill any time PostgreSQL databases need to be queried, database schemas need to be inspected, SQL needs to be executed via MCP, or database state needs to be managed. Trigger immediately on: \"PostgreSQL\", \"postgres\", \"query database\", \"SQL query\", \"database MCP\", \"run SQL\", \"table schema\", \"database table\", \"Postgres MCP\", \"execute query\", \"inspect database\", \"database connection\", \"pg query\", \"Supabase SQL\". If someone says \"query the database\" or \"run this SQL\" this skill MUST trigger."
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
