---
name: llamaindex-tools
description: 'RAG and document intelligence toolkit for querying LlamaCloud indexes and extracting structured data from documents using Llama. Triggers: "use llamaindex-tools", "llamaindex tools", "llamaindex task.'
allowed-tools: Bash, Glob, Grep, Read
---

# LlamaIndex Tools

Query LlamaCloud indexes and extract data via the `llamaindex` MCP server.

## Core Capabilities

| Capability | Description |
|-----------|-------------|
| **Index Querying** | Search one or more LlamaCloud indexes with natural language |
| **Data Extraction** | Use extract agents to pull structured data from files |
| **Multi-Index** | Define multiple indexes, each becomes a separate tool |

## How It Works

The `llamacloud-mcp` server creates one MCP tool per index you define. Each tool accepts a `query` parameter and returns results from that specific LlamaCloud index. Tool names are auto-generated as `get_information_<index_name>`.

## Configuration

### CLI Usage
```bash
uvx llamacloud-mcp@latest \
  --index "my-docs:Search the documentation index" \
  --index "my-data:Search the data analysis index" \
  --extract-agent "extractor:Extract structured data from documents" \
  --project-name "My Project" \
  --org-id "your-org-id" \
  --api-key "your-api-key"
```

### CLI Options
- `--index TEXT` — Index definition as `name:description` (repeatable)
- `--extract-agent TEXT` — Extract agent as `name:description` (repeatable)
- `--project-id TEXT` — LlamaCloud project ID
- `--org-id TEXT` — LlamaCloud organization ID
- `--api-key TEXT` — LlamaCloud API key
- `--transport` — `stdio`, `sse`, or `streamable-http` (default: stdio)

## Setup Requirements

1. Create a [LlamaCloud](https://cloud.llamaindex.ai/) account → verify: output exists + parses without error
2. Create an index with your data sources (Google Drive, uploaded docs, etc.) → verify: file content matches expected shape
3. Get an API key from the LlamaCloud UI → verify: step output matches expected outcome
4. Set `LLAMA_CLOUD_API_KEY` in your environment or pass via `--api-key` → verify: step output matches expected outcome

## Common Workflows

### Query a Knowledge Base
```
User: "What does our documentation say about authentication?"
-> Tool: get_information_my_docs({ query: "authentication" })
-> Returns: Relevant passages from the indexed documentation
```

### Multi-Index Search
Define separate indexes for different data sources:
```bash
--index "docs:Product documentation"
--index "support:Customer support tickets"
--index "research:Internal research papers"
```

### Extract Structured Data
Use extract agents to pull specific data from documents:
```bash
--extract-agent "invoice-extractor:Extract line items, totals, and dates from invoices"
```

## LlamaIndex as MCP Client

LlamaIndex also works as an MCP client — you can convert any MCP server's tools into LlamaIndex `FunctionTool` objects:

```python
from llama_index.tools.mcp import BasicMCPClient, McpToolSpec

mcp_client = BasicMCPClient("http://localhost:8000/sse")
tool_spec = McpToolSpec(client=mcp_client)
tools = tool_spec.to_tool_list()
```

## Gotchas
- Requires `LLAMA_CLOUD_API_KEY` — get from https://cloud.llamaindex.ai/
- Each `--index` creates a separate MCP tool named `get_information_<index_name>`
- Indexes must be created and populated in LlamaCloud UI before querying
- Default project is "Default" if `--project-name` is not specified
- Supports stdio, SSE, and streamable-http transports
- Extract agents are separate from index queries — use for structured data extraction

## When NOT to use

- Task is unrelated to llamaindex tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Llamaindex Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for llamaindex tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving llamaindex tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
