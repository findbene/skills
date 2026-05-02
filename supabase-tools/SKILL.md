---
name: supabase-tools
description: "Supabase database, auth, edge functions, storage, and real-time management via MCP for full Supabase project operations. Use this skill any time Supabase projects need to be managed, database migrations need to be run, RLS policies need to be applied, edge functions need to be deployed, or Supabase data needs to be queried. Trigger immediately on: \"Supabase\", \"Supabase project\", \"Supabase migration\", \"RLS\", \"row level security\", \"edge function\", \"Supabase auth\", \"Supabase storage\", \"Supabase query\", \"pgvector Supabase\", \"Supabase bucket\", \"Supabase table\", \"Supabase MCP\", \"Realtime\". If someone says \"manage my Supabase project\" or \"deploy a Supabase edge function\" this skill MUST trigger."
---

# Supabase Tools

Manage Supabase projects — database, migrations, edge functions, and infrastructure.

## Available Tools

### Database
| Tool | Purpose |
|------|---------|
| `execute_sql` | Run SQL queries against the database |
| `list_tables` | View all tables and their schemas |
| `apply_migration` | Create and apply a database migration |
| `list_migrations` | View migration history |
| `list_extensions` | List installed Postgres extensions |
| `get_advisors` | Get database optimization recommendations |

### Edge Functions
| Tool | Purpose |
|------|---------|
| `deploy_edge_function` | Deploy a Deno edge function |
| `get_edge_function` | Get function details and code |
| `list_edge_functions` | List all deployed functions |

### Project Management
| Tool | Purpose |
|------|---------|
| `list_projects` | View all Supabase projects |
| `get_project` | Get project details and status |
| `create_project` | Create a new Supabase project |
| `pause_project` / `restore_project` | Manage project lifecycle |
| `get_project_url` | Get the project's API URL |
| `get_publishable_keys` | Get anon/service role keys |

### Infrastructure
| Tool | Purpose |
|------|---------|
| `list_organizations` | List your organizations |
| `get_organization` | Get org details |
| `get_cost` / `confirm_cost` | Check and confirm costs |
| `get_logs` | Read application/database logs |
| `generate_typescript_types` | Generate TypeScript types from schema |

### Branching (Preview)
| Tool | Purpose |
|------|---------|
| `create_branch` | Create a database branch |
| `list_branches` | View all branches |
| `merge_branch` | Merge branch to production |
| `reset_branch` / `rebase_branch` | Reset or rebase a branch |

## Common Workflows

### Run a Query
```
execute_sql(project_id="...", query="SELECT * FROM users LIMIT 10")
```

### Create a Migration
```
apply_migration(
  project_id="...",
  name="add_rsvp_table",
  query="CREATE TABLE rsvp (id uuid PRIMARY KEY DEFAULT gen_random_uuid(), ...)"
)
```

### Deploy an Edge Function
```
deploy_edge_function(
  project_id="...",
  name="send-confirmation",
  code="import { serve } from 'https://deno.land/std/http/server.ts'..."
)
```

### Generate Types for Frontend
```
generate_typescript_types(project_id="...")
```
Returns TypeScript interfaces matching your database schema — paste into your project's `types/supabase.ts`.

### Check Project Health
1. `get_project(project_id="...")` for status
2. `get_logs(project_id="...", service="api")` for recent API logs
3. `get_advisors(project_id="...")` for optimization suggestions

## Tips

- Always use `apply_migration` for schema changes, not raw `execute_sql` — migrations are versioned and reversible
- `generate_typescript_types` keeps your frontend type-safe with the database
- Use branching for testing schema changes before applying to production
- `get_advisors` catches missing indexes and other performance issues
