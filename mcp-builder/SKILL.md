---
name: mcp-builder
description: 'Expert guide for building MCP (Model Context Protocol) servers that expose tools and resources to Claude and other LLMs. Triggers: "use mcp-builder", "use MCP builder", "mcp builder".'
allowed-tools: Bash, Glob, Grep, Read
---

# MCP Server Builder

Create MCP servers enabling LLMs to interact with external services.

## Process Overview

### Phase 1: Research & Planning
1. Study MCP Protocol: `https://modelcontextprotocol.io/sitemap.xml` → verify: step output matches expected outcome
2. Choose stack: TypeScript (recommended) or Python → verify: step output matches expected outcome
3. Plan tool coverage: Map API endpoints to tools → verify: step output matches expected outcome

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

## When NOT to use

- Task is unrelated to mcp builder — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Mcp Builder needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for mcp builder
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving mcp builder
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
