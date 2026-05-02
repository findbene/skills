---
name: crewai-tools
description: "CrewAI multi-agent workflow orchestration via MCP for running, creating, and managing CrewAI crews and tasks. Use this skill any time CrewAI agents need to be orchestrated, multi-agent crews need to be built, or CrewAI workflows need to be run via MCP tools. Trigger immediately on: \"CrewAI\", \"crew\", \"crewai\", \"CrewAI workflow\", \"multi-agent crew\", \"CrewAI task\", \"CrewAI agent\", \"crewai MCP\", \"run crew\", \"CrewAI orchestration\", \"agent crew\", \"build a crew\", \"CrewAI tools\". If someone says \"build a CrewAI workflow\" or \"run this with CrewAI\" this skill MUST trigger."
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
1. Define researcher and writer agents
2. Create research task assigned to researcher
3. Create writing task assigned to writer (uses research output)
4. Run sequentially

### Code Review Team
1. Define architect, developer, reviewer agents
2. Create design, implementation, review tasks
3. Run hierarchically with architect as manager

## Gotchas
- Requires agents.yml and tasks.yml files — the MCP server loads these at startup
- Template variables use `{variable_name}` syntax in YAML files
- Sequential process runs tasks in order; hierarchical requires a manager agent
- The server runs in STDIO mode for local development
- Requires Python 3.11+ and CrewAI installed
