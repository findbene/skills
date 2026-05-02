---
name: llamaindex-tools
description: "RAG and document intelligence toolkit for querying LlamaCloud indexes and extracting structured data from documents using LlamaIndex via MCP. Use this skill any time a LlamaCloud index needs to be queried, RAG pipelines need to be built, structured data needs to be extracted from documents, or knowledge base search is needed. Trigger immediately on: \"LlamaIndex\", \"LlamaCloud\", \"RAG pipeline\", \"query index\", \"document extraction\", \"knowledge base search\", \"retrieval augmented generation\", \"llamaindex\", \"semantic search\", \"document Q&A\", \"LlamaIndex MCP\", \"index search\". If someone says \"query my LlamaCloud index\" or \"search my knowledge base\" this skill MUST trigger."
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

1. Create a [LlamaCloud](https://cloud.llamaindex.ai/) account
2. Create an index with your data sources (Google Drive, uploaded docs, etc.)
3. Get an API key from the LlamaCloud UI
4. Set `LLAMA_CLOUD_API_KEY` in your environment or pass via `--api-key`

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
