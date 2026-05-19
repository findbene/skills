---
name: huggingface-tools
description: 'Hugging Face Hub exploration via MCP for searching models, datasets, Spaces, and papers across the ML ecosystem. Triggers: "use huggingface-tools", "huggingface tools", "huggingface task".'
allowed-tools: Bash, Glob, Grep, Read
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
1. Search models by task type (e.g., "text-generation", "image-classification") → verify: step output matches expected outcome
2. Filter by framework (PyTorch, TensorFlow, JAX) → verify: step output matches expected outcome
3. Get model card details and usage examples → verify: step output matches expected outcome

### Run a Community Tool
1. Enable a Gradio Space in HF settings → verify: step output matches expected outcome
2. The Space's tools appear automatically as MCP tools → verify: step output matches expected outcome
3. Call the tool with appropriate parameters → verify: step output matches expected outcome

### Research ML Papers
1. Search papers by topic or author → verify: step output matches expected outcome
2. Get paper details, abstracts, and related models → verify: step output matches expected outcome

## Gotchas
- Tools are dynamic — configure which ones are active at https://huggingface.co/settings/mcp
- `HF_TOKEN` is required for authenticated access (private models, higher rate limits)
- Gradio Spaces may have cold start delays (10-30s) if not recently used
- Add `?no_image_content=true` to URL to remove image blocks from Gradio responses
- The server supports STDIO, SSE, and Streamable HTTP transports

## When NOT to use

- Task doesn't involve Hugging Face hub queries and model usage → use the matching domain skill instead
- Simple one-off operation that doesn't need this skill's structure
- Different toolchain required → check `find-skills` skill for alternatives
- User explicitly asks to skip skill discipline → respect the override

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll skip the verify step, output looks right" | Eyeballing without verification ships broken outputs |
| "Generic answer is good enough" | Hugging Face workflows needs domain-specific decisions, not boilerplate |
| "I can hold all context in head" | Multi-step state loss creates regressions you won't catch |
| "Just one more shortcut" | Shortcuts compound — finish discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced and matches user's stated goal
- All verification steps in process passed
- Edge cases for Hugging Face hub queries and model usage addressed or explicitly noted
- Output is reproducible (no hidden state)
- Hand-off summary provided so user can validate without re-reading entire flow

## Examples

### Example 1 — golden path
- Input: standard request involving Hugging Face hub queries and model usage
- Action: follow the documented numbered process, apply verify clauses per step
- Output: deliverable that passes the Output Contract

### Example 2 — edge case
- Input: request with non-standard constraint or partial info
- Action: detect the gap, ask clarifying question OR document assumption, proceed with adapted process
- Output: deliverable + explicit note on assumption/limitation
