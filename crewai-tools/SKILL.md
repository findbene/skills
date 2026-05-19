---
name: crewai-tools
description: 'CrewAI multi-agent workflow orchestration via MCP for running, creating, and managing CrewAI crews and tasks. Triggers: "use crewai-tools", "crewai tools", "crewai task".'
allowed-tools: Bash, Glob, Grep, Read
---

# CrewAI Tools

Orchestrate multi-agent AI workflows via the `crewai` MCP server.

## Available Tools

| Tool | Purpose |
|------|---------|
| `run_workflow` | Execute a pre-configured CrewAI workflow from agents.yml and tasks.yml |

## How CrewAI Works

CrewAI organizes work into **Crews** composed of:
- **Agents** — specialized roles with goals and backstories (defined in `agents.yml`)
- **Tasks** — work items with descriptions, expected outputs, and agent assignments (defined in `tasks.yml`)
- **Process** — sequential (tasks run in order) or hierarchical (manager delegates)

## Configuration Files

### agents.yml
```yaml
researcher:
  role: Research Analyst
  goal: Find comprehensive information on the given topic
  backstory: >
    You are an expert researcher with a keen eye for detail...

writer:
  role: Content Writer
  goal: Create engaging content based on research findings
  backstory: >
    You are a skilled writer who transforms research into compelling narratives...
```

### tasks.yml
```yaml
research_task:
  description: >
    Research {topic} thoroughly, covering key aspects and recent developments.
  expected_output: Detailed research report with sources
  agent: researcher

writing_task:
  description: >
    Write an engaging article based on the research findings.
  expected_output: Publication-ready article
  agent: writer
  output_file: output.md
```

## Running Workflows

```bash
# Via uvx (recommended)
uvx mcp-crew-ai --agents path/to/agents.yml --tasks path/to/tasks.yml --topic "Your Topic"

# With process type
uvx mcp-crew-ai --agents agents.yml --tasks tasks.yml --process hierarchical --verbose

# With custom variables
uvx mcp-crew-ai --agents agents.yml --tasks tasks.yml --variables '{"year": 2026, "focus": "AI"}'
```

## CLI Options

- `--agents` — path to agents YAML (required)
- `--tasks` — path to tasks YAML (required)
- `--topic` — main topic for the crew (default: "Artificial Intelligence")
- `--process` — "sequential" or "hierarchical" (default: sequential)
- `--verbose` — enable verbose output
- `--variables` — JSON string or file path with template variables

## Common Workflow Patterns

### Research + Writing Pipeline
1. Define researcher and writer agents → verify: output file exists + no syntax error
2. Create research task assigned to researcher → verify: output file exists + no syntax error
3. Create writing task assigned to writer (uses research output) → verify: output file exists + no syntax error
4. Run sequentially → verify: command exit code 0

### Code Review Team
1. Define architect, developer, reviewer agents → verify: step output matches expected outcome
2. Create design, implementation, review tasks → verify: output file exists + no syntax error
3. Run hierarchically with architect as manager → verify: command exit code 0

## Gotchas
- Requires agents.yml and tasks.yml files — the MCP server loads these at startup
- Template variables use `{variable_name}` syntax in YAML files
- Sequential process runs tasks in order; hierarchical requires a manager agent
- The server runs in STDIO mode for local development
- Requires Python 3.11+ and CrewAI installed

## When NOT to use

- Task is unrelated to crewai tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Crewai Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for crewai tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving crewai tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
