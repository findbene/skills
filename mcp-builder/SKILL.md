---
name: mcp-builder
description: "Expert guide for building MCP (Model Context Protocol) servers that expose tools and resources to Claude and other LLMs. Use this skill any time a new MCP server needs to be built, an API needs to be wrapped as MCP tools, Claude needs custom tools integrated, or an external service needs to be made available via MCP. Trigger immediately on: \"MCP server\", \"build MCP\", \"Model Context Protocol\", \"custom tools for Claude\", \"wrap API as MCP\", \"MCP tools\", \"MCP resources\", \"create MCP\", \"expose API to Claude\", \"MCP integration\", \"tool server\", \"MCP SDK\", \"tools for LLM\". If someone says \"I want Claude to call my API\" or \"build me an MCP server\" this skill MUST trigger."
---

# MCP Server Builder

Create MCP servers enabling LLMs to interact with external services.

## Process Overview

### Phase 1: Research & Planning
1. Study MCP Protocol: `https://modelcontextprotocol.io/sitemap.xml`
2. Choose stack: TypeScript (recommended) or Python
3. Plan tool coverage: Map API endpoints to tools

### Phase 2: Implementation

#### Project Structure (TypeScript)
```
my-mcp-server/
├── src/
│   ├── index.ts        # Server entry point
│   ├── tools/          # Tool implementations
│   └── utils/          # Shared utilities
├── package.json
└── tsconfig.json
```

#### Tool Implementation
```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { z } from "zod";

const server = new McpServer({ name: "my-server", version: "1.0.0" });

server.tool(
  "get_resource",
  "Fetch a resource by ID",
  { id: z.string().describe("Resource identifier") },
  async ({ id }) => {
    const result = await fetchResource(id);
    return { content: [{ type: "text", text: JSON.stringify(result) }] };
  }
);
```

### Tool Design Best Practices

**Naming:** Clear, action-oriented (`github_create_issue`, `slack_send_message`)

**Input Schema:** Use Zod (TS) or Pydantic (Python) with clear descriptions, constraints, and examples.

**Annotations:** `readOnlyHint`, `destructiveHint`, `idempotentHint`

**Error Messages:** Actionable with specific suggestions.

### Phase 3: Testing
```bash
npm run build
npx @modelcontextprotocol/inspector
```

### Phase 4: Evaluations
Create 10 test questions that are independent, read-only, require multiple tool calls, and have verifiable answers.

## Transport Options
- **Streamable HTTP:** Remote servers (stateless JSON)
- **stdio:** Local servers
