---
name: mineru
description: Convert PDF, DOCX, PPTX, XLSX, and image files to Markdown or JSON using MinerU. Use when the user wants to parse documents, extract text/tables/formulas, or prepare documents for RAG/LLM pipelines.
---

# MinerU — Document Parsing Skill

MinerU converts PDF, DOCX, PPTX, XLSX, and images → Markdown + JSON.
Repo: `C:\Projects\MinerU` | Python package: `mineru` | CLI: `mineru`

## Quick Usage

### CLI (primary method)
```bash
# Pipeline backend (CPU-only, works everywhere, 85+ accuracy)
mineru -p <input_path> -o <output_dir> -b pipeline

# Default backend (GPU if available, else falls back)
mineru -p <input_path> -o <output_dir>

# Directory of files
mineru -p C:\docs\ -o C:\parsed\

# Specific format
mineru -p doc.pdf -o out/ -b pipeline
```

### FastAPI server
```bash
mineru-api --host 127.0.0.1 --port 8000
# Then POST to http://127.0.0.1:8000/file_parse or /tasks
```

### Gradio WebUI
```bash
mineru-gradio --server-name 127.0.0.1 --server-port 7860
```

## Output Files

MinerU writes to `<output_dir>/<filename>/`:
- `<filename>.md` — Markdown with text, tables (HTML), formulas (LaTeX)
- `<filename>_middle.json` — structured intermediate representation
- `<filename>_content_list.json` — flat content list (blocks)
- `images/` — extracted images
- `<filename>_layout.pdf` — layout visualization (optional)

## Python API

```python
from mineru.api import parse_document

# Returns (markdown_str, content_list)
md, content = parse_document("path/to/file.pdf", backend="pipeline")
```

Or via subprocess if mineru is installed system-wide:
```python
import subprocess, json, pathlib

def parse_to_markdown(input_path: str, output_dir: str) -> str:
    subprocess.run(
        ["mineru", "-p", input_path, "-o", output_dir, "-b", "pipeline"],
        check=True
    )
    stem = pathlib.Path(input_path).stem
    md_file = pathlib.Path(output_dir) / stem / f"{stem}.md"
    return md_file.read_text(encoding="utf-8")
```

## FastAPI Async Example (Python)

```python
import httpx, asyncio

async def parse_async(file_path: str, api_url="http://127.0.0.1:8000"):
    async with httpx.AsyncClient(timeout=120) as client:
        with open(file_path, "rb") as f:
            r = await client.post(f"{api_url}/tasks",
                files={"files": f},
                data={"return_md": "true"})
        task_id = r.json()["task_id"]
        while True:
            status = await client.get(f"{api_url}/tasks/{task_id}")
            if status.json()["status"] in ("completed", "failed"):
                break
            await asyncio.sleep(2)
        return (await client.get(f"{api_url}/tasks/{task_id}/result")).json()
```

## MCP Server (MinerU Document Explorer)

Installed via: `npm install -g mineru-document-explorer`
MCP command: `qmd mcp`

Add to `~/.claude/settings.json` → `mcpServers`:
```json
"mineru": {
  "command": "qmd",
  "args": ["mcp"]
}
```

Tools exposed: document retrieval, deep reading, knowledge ingestion (15 tools).

## Backends

| Backend | Accuracy | Needs GPU | Notes |
|---------|----------|-----------|-------|
| `pipeline` | 85+ | No (CPU OK) | Fast, stable, Windows-safe |
| `vlm-auto-engine` | 95+ | Yes (8GB VRAM) | High accuracy |
| `hybrid-auto-engine` | 95+ | Yes (8GB VRAM) | Best for complex layouts |
| `vlm-http-client` | 95+ | No (remote) | Connects to OpenAI-compatible server |
| `hybrid-http-client` | 95+ | No (remote) | Needs pipeline deps locally |

## Model Source

Default: HuggingFace. Switch to ModelScope if HF blocked:
```bash
# PowerShell
$env:MINERU_MODEL_SOURCE = "modelscope"
```

## Config File

`~\mineru.json` (auto-created by `mineru-models-download`):
```json
{
  "models-dir": {
    "pipeline": "C:/Users/findb/.cache/mineru/models/pipeline",
    "vlm": "C:/Users/findb/.cache/mineru/models/vlm"
  },
  "latex-delimiter-config": "$"
}
```

## When to Use Which Backend

- **User has no GPU or just wants it to work**: `pipeline`
- **User wants max accuracy, has GPU**: `vlm-auto-engine` or `hybrid-auto-engine`
- **Running on edge device, server elsewhere**: `vlm-http-client`
- **Batch processing, high throughput**: `mineru-api` + `POST /tasks` (async)
- **Interactive UI**: `mineru-gradio`
