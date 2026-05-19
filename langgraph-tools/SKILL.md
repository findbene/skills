---
name: langgraph-tools
description: 'LangGraph and LangChain multi-agent workflow construction via MCP for building stateful agent pipelines and graph-based workflows. Triggers: "use langgraph-tools", "langgraph tools", "langgraph task".'
allowed-tools: Bash, Glob, Grep, Read
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
1. Define MCP server connections in a config dict → verify: step output matches expected outcome
2. Use `MultiServerMCPClient` to load tools from all servers → verify: file content matches expected shape
3. Pass tools to `create_react_agent` or a custom LangGraph graph → verify: output exists + parses without error
4. Run the agent with user messages → verify: command exit code 0

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

## When NOT to use

- Task is unrelated to langgraph tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Langgraph Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for langgraph tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving langgraph tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
