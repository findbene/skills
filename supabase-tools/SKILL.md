---
name: supabase-tools
description: 'Supabase database, auth, edge functions, storage, and real-time management via MCP for full Supabase project operations. Triggers: "use supabase-tools", "supabase tools", "supabase task".'
allowed-tools: Glob, Grep, Read
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
1. `get_project(project_id="...")` for status → verify: step output matches expected outcome
2. `get_logs(project_id="...", service="api")` for recent API logs → verify: step output matches expected outcome
3. `get_advisors(project_id="...")` for optimization suggestions → verify: step output matches expected outcome

## Tips

- Always use `apply_migration` for schema changes, not raw `execute_sql` — migrations are versioned and reversible
- `generate_typescript_types` keeps your frontend type-safe with the database
- Use branching for testing schema changes before applying to production
- `get_advisors` catches missing indexes and other performance issues

## When NOT to use

- Task is unrelated to supabase tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Supabase Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for supabase tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving supabase tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
