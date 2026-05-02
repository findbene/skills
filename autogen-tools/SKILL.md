---
name: autogen-tools
description: "Microsoft AutoGen multi-agent conversation orchestration via MCP for building conversational AI agents, code-executing agents, and multi-agent group chats. Use this skill any time AutoGen agents need to be created, multi-agent conversations need to be orchestrated, or code-executing AI agents need to be built with AutoGen. Trigger immediately on: \"AutoGen\", \"autogen\", \"multi-agent conversation\", \"AutoGen workflow\", \"conversational agent\", \"code execution agent\", \"AutoGen group chat\", \"AutoGen MCP\", \"Microsoft AutoGen\", \"agent conversation\", \"AutoGen agent\", \"AutoGen tool\". If someone says \"build an AutoGen agent\" or \"orchestrate agents with AutoGen\" this skill MUST trigger."
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
