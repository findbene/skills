---
name: autogen-tools
description: 'Microsoft AutoGen multi-agent conversation orchestration via MCP for building conversational AI agents, code-executing agents, and multi. Triggers: "use autogen-tools", "autogen tools", "autogen task.'
allowed-tools: Glob, Grep, Read
---

# AutoGen Tools

Orchestrate Microsoft AutoGen multi-agent systems via the `autogen` MCP server.

## Available Tools

### Core Agent Management
| Tool | Purpose |
|------|---------|
| `create_agent` | Create agents with name, type, and system message |
| `create_workflow` | Build complete multi-agent workflow configurations |
| `get_agent_status` | Get detailed metrics and health for an agent |

### Conversation Execution
| Tool | Purpose |
|------|---------|
| `execute_chat` | Two-agent conversation (initiator + responder) |
| `execute_group_chat` | Multi-agent group discussion |
| `execute_nested_chat` | Hierarchical conversation structures |
| `execute_swarm` | Swarm-based collaborative problem solving |

### Workflow Orchestration
| Tool | Purpose |
|------|---------|
| `execute_workflow` | Run predefined workflow templates |
| `manage_agent_memory` | Handle agent learning and persistence |
| `configure_teachability` | Enable agent learning capabilities |

## Built-in Workflow Templates

| Workflow | Pipeline | Use Case |
|----------|----------|----------|
| `code_generation` | Architect > Developer > Reviewer > Executor | Build and validate code |
| `research` | Researcher > Analyst > Critic > Synthesizer | Deep topic analysis |
| `creative_writing` | Multi-stage creative collaboration | Content creation |
| `problem_solving` | Structured approach to complex problems | Decision making |
| `code_review` | Security > Performance > Style teams | Code quality |

## Agent Types

- **Assistant** — LLM-powered conversational agents
- **User Proxy** — code execution and human interaction
- **Conversable** — flexible conversation patterns
- **Teachable** — learning and memory persistence
- **Retrievable** — knowledge base integration

## Common Workflows

### Create Agents and Chat
```json
// 1. Create agents
create_agent({ name: "researcher", type: "assistant", system_message: "You are a research specialist." })
create_agent({ name: "analyst", type: "assistant", system_message: "You analyze research findings." })

// 2. Execute chat
execute_chat({ initiator: "researcher", responder: "analyst", message: "Analyze the latest AI trends." })
```

### Run a Predefined Workflow
```json
execute_workflow({
  workflow_name: "code_generation",
  input_data: { task: "Create a REST API", language: "python", requirements: ["FastAPI", "Pydantic"] },
  quality_checks: true
})
```

### Group Chat with Multiple Agents
```json
execute_group_chat({
  agents: ["architect", "developer", "reviewer"],
  message: "Design a microservices architecture for an e-commerce platform."
})
```

## Available Prompts
- `autogen-workflow` — create multi-agent workflows (args: task_description, agent_count, workflow_type)
- `code-review` — collaborative code review (args: code, language, focus_areas)
- `research-analysis` — research teams (args: topic, depth)

## Available Resources
- `autogen://agents/list` — active agents with status
- `autogen://workflows/templates` — available workflow templates
- `autogen://chat/history` — recent conversation history
- `autogen://config/current` — server configuration

## Gotchas
- Requires `OPENAI_API_KEY` env var (AutoGen uses OpenAI models by default)
- Create agents before attempting chats — agent names must match exactly
- Group chats support speaker selection modes: auto, manual, random, round-robin
- Teachable agents persist knowledge across sessions
- Quality checks increase execution time but improve output reliability
- Swarm mode is experimental
- Node.js 18+ and Python dependencies both required

## When NOT to use

- Task is unrelated to autogen tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Autogen Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for autogen tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving autogen tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
