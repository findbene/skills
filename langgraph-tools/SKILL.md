---
name: langgraph-tools
description: "LangGraph and LangChain multi-agent workflow construction via MCP for building stateful agent pipelines and graph-based workflows. Use this skill any time LangGraph agents need to be built, LangChain tools need to be bridged via MCP, stateful agent workflows need to be created, or LangGraph state machines need to be designed. Trigger immediately on: \"LangGraph\", \"LangChain\", \"LangGraph agent\", \"state machine\", \"LangGraph workflow\", \"LangChain tool\", \"graph workflow\", \"LangGraph MCP\", \"agent pipeline\", \"LangGraph state\", \"LangChain agent\", \"LangGraph node\", \"conditional graph\". If someone says \"build a LangGraph workflow\" or \"create a LangChain agent\" this skill MUST trigger."
---

# LangGraph Tools

Bridge MCP tools into LangChain/LangGraph agents via `langchain-mcp-adapters`.

## What This Is

`langchain-mcp-adapters` is the official LangChain library that converts MCP server tools into LangChain-compatible tools usable by LangGraph agents. It is a **client adapter**, not an MCP server — it lets LangGraph agents **consume** tools from any MCP server.

Location: `~/.claude/mcp-server-langgraph/`

## Installation

```bash
pip install langchain-mcp-adapters langgraph "langchain[openai]"
```

## Core Components

### MultiServerMCPClient
Connects to one or more MCP servers and loads their tools.

```python
from langchain_mcp_adapters.client import MultiServerMCPClient

async with MultiServerMCPClient({
    "math": {
        "command": "python",
        "args": ["math_server.py"],
        "transport": "stdio"
    },
    "weather": {
        "url": "http://localhost:8000/mcp",
        "transport": "streamable_http"
    }
}) as client:
    tools = client.get_tools()
```

### Using with LangGraph Agent

```python
from langgraph.prebuilt import create_react_agent
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model="gpt-4o")

async with MultiServerMCPClient(server_configs) as client:
    tools = client.get_tools()
    agent = create_react_agent(model, tools)
    result = await agent.ainvoke({"messages": [("user", "What is 2+2?")]})
```

## Transport Options

| Transport | Config Key | Use Case |
|-----------|-----------|----------|
| STDIO | `"transport": "stdio"` | Local servers, subprocess |
| Streamable HTTP | `"transport": "streamable_http"` | Remote/web servers |
| SSE | `"transport": "sse"` | Server-Sent Events |

## Common Patterns

### Connect LangGraph to Multiple MCP Servers
```python
server_configs = {
    "filesystem": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-filesystem", "/tmp"],
        "transport": "stdio"
    },
    "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "transport": "stdio",
        "env": {"GITHUB_TOKEN": "ghp_xxx"}
    }
}
```

### Create a Custom LangGraph Agent with MCP Tools
1. Define MCP server connections in a config dict
2. Use `MultiServerMCPClient` to load tools from all servers
3. Pass tools to `create_react_agent` or a custom LangGraph graph
4. Run the agent with user messages

## Integration with Claude Code

Since this is a client library (not an MCP server), it doesn't appear in settings.json. Use it when:
- Building Python applications that need LangGraph agents with MCP tool access
- Creating agentic pipelines that bridge LangChain and MCP ecosystems
- Connecting existing MCP servers to LangGraph-based applications

## Gotchas
- This is a client adapter, NOT an MCP server — it consumes tools, doesn't provide them
- Requires an LLM API key (OpenAI, Anthropic, etc.) for the LangGraph agent
- Each `MultiServerMCPClient` context manager starts/stops MCP server subprocesses
- STDIO transport spawns the server as a subprocess; HTTP connects to running servers
- Tool schemas are automatically converted from MCP format to LangChain format
