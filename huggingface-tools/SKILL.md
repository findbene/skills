---
name: huggingface-tools
description: "Hugging Face Hub exploration via MCP for searching models, datasets, Spaces, and papers across the ML ecosystem. Use this skill any time Hugging Face models need to be searched, datasets need to be found, ML model cards need to be inspected, or AI model repositories need to be explored. Trigger immediately on: \"Hugging Face\", \"HuggingFace\", \"HF model\", \"search models\", \"model hub\", \"dataset search\", \"Spaces\", \"model card\", \"transformers model\", \"HF repo\", \"huggingface\", \"model search\", \"AI model search\", \"HF MCP\". If someone says \"find a model on Hugging Face\" or \"search HuggingFace for X\" this skill MUST trigger."
---

# HuggingFace Tools

Access the Hugging Face Hub ecosystem via the `huggingface` MCP server.

## Core Capabilities

| Category | What You Can Do |
|----------|----------------|
| **Models** | Search, explore, and get details on 500K+ ML models |
| **Datasets** | Find and explore datasets for training and evaluation |
| **Spaces** | Discover and interact with Gradio/Streamlit demo apps |
| **Papers** | Search ML research papers on the Hub |
| **Gradio Tools** | Run community AI tools hosted as Gradio apps on Spaces |

## How It Works

The HF MCP Server connects to `https://huggingface.co/mcp` and provides tools dynamically based on your configuration at https://huggingface.co/settings/mcp. You can enable/disable specific tools and Spaces.

## Available Tool Categories

### Hub Search Tools
- Search models by task, framework, or keyword
- Search datasets by task, size, or keyword
- Search Spaces by functionality
- Search papers by topic

### Hub Documentation Tools
- `hf_doc_search` — search HF documentation
- `hf_doc_fetch` — fetch specific documentation pages

### Gradio Space Tools
- Any Gradio Space can be exposed as an MCP tool
- Configure which Spaces to enable at https://huggingface.co/settings/mcp

## Setup

### Authentication
Get a token from https://huggingface.co/settings/tokens and set it:
- In settings.json as `HF_TOKEN` env var
- Or via Claude Code: `claude mcp add hf-mcp-server -t http https://huggingface.co/mcp -H "Authorization: Bearer YOUR_TOKEN"`

### Transport Modes
```bash
npx @llmindset/hf-mcp-server       # STDIO mode (local)
npx @llmindset/hf-mcp-server-http  # Streamable HTTP mode
npx @llmindset/hf-mcp-server-json  # Streamable HTTP JSON mode
```

## Common Workflows

### Find a Model for a Task
1. Search models by task type (e.g., "text-generation", "image-classification")
2. Filter by framework (PyTorch, TensorFlow, JAX)
3. Get model card details and usage examples

### Run a Community Tool
1. Enable a Gradio Space in HF settings
2. The Space's tools appear automatically as MCP tools
3. Call the tool with appropriate parameters

### Research ML Papers
1. Search papers by topic or author
2. Get paper details, abstracts, and related models

## Gotchas
- Tools are dynamic — configure which ones are active at https://huggingface.co/settings/mcp
- `HF_TOKEN` is required for authenticated access (private models, higher rate limits)
- Gradio Spaces may have cold start delays (10-30s) if not recently used
- Add `?no_image_content=true` to URL to remove image blocks from Gradio responses
- The server supports STDIO, SSE, and Streamable HTTP transports
