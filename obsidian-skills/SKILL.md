---
name: obsidian-skills
description: >
  Work with Obsidian vaults and files — Obsidian Flavored Markdown (.md), Bases database views (.base),
  JSON Canvas mind maps (.canvas), and the Obsidian CLI for vault automation and plugin development.
  Also includes Defuddle for extracting clean markdown from web pages (prefer over WebFetch for articles).
  Use this skill whenever the user is working inside an Obsidian vault, mentions wikilinks, callouts,
  embeds, frontmatter, .base files, .canvas files, Bases formulas/views/filters, or asks to read a URL
  cleanly. Trigger immediately on: wikilinks, [[note]], ![[embed]], callouts, Obsidian properties,
  .base file, .canvas file, Bases, JSON Canvas, obsidian cli, defuddle, vault, parse URL.
---

# Obsidian Skills

Five integrated capabilities for working with Obsidian vaults and files.

## Capability Map

| When you need to... | Use |
|---------------------|-----|
| Create/edit `.md` files with wikilinks, callouts, embeds, properties | → Read [`references/obsidian-markdown.md`](references/obsidian-markdown.md) |
| Create/edit `.base` database views with filters, formulas, summaries | → Read [`references/obsidian-bases.md`](references/obsidian-bases.md) |
| Create/edit `.canvas` mind maps or flowcharts | → Read [`references/json-canvas.md`](references/json-canvas.md) |
| Automate vault operations or develop plugins via CLI | → See **Obsidian CLI** section below |
| Extract clean markdown from a web URL | → See **Defuddle** section below |

---

## Obsidian CLI

Use the `obsidian` CLI to interact with a running Obsidian instance. Requires Obsidian to be open.

```bash
obsidian help          # always up-to-date command reference
```

Full docs: https://help.obsidian.md/cli

### Syntax

**Parameters** take a value with `=`. Quote values with spaces:
```bash
obsidian create name="My Note" content="Hello world"
```

**Flags** are boolean switches:
```bash
obsidian create name="My Note" silent overwrite
```

Use `\n` for newlines and `\t` for tabs in multiline content.

### File targeting

- `file=<name>` — resolves like a wikilink (name only, no path or extension)
- `path=<path>` — exact path from vault root, e.g. `folder/note.md`
- Without either → targets the active file

### Vault targeting

```bash
obsidian vault="My Vault" search query="test"   # target specific vault
```

### Common patterns

```bash
obsidian read file="My Note"
obsidian create name="New Note" content="# Hello" template="Template" silent
obsidian append file="My Note" content="New line"
obsidian search query="search term" limit=10
obsidian daily:read
obsidian daily:append content="- [ ] New task"
obsidian property:set name="status" value="done" file="My Note"
obsidian tasks daily todo
obsidian tags sort=count counts
obsidian backlinks file="My Note"
```

Use `--copy` on any command to copy output to clipboard. Use `silent` to suppress file opening. Use `total` on list commands for a count.

### Plugin development workflow

After changing plugin or theme code:

1. Reload the plugin:
   ```bash
   obsidian plugin:reload id=my-plugin
   ```
2. Check for errors:
   ```bash
   obsidian dev:errors
   ```
3. Verify visually:
   ```bash
   obsidian dev:screenshot path=screenshot.png
   obsidian dev:dom selector=".workspace-leaf" text
   ```
4. Check console:
   ```bash
   obsidian dev:console level=error
   ```

Additional developer commands:
```bash
obsidian eval code="app.vault.getFiles().length"   # run JS in app context
obsidian dev:css selector=".workspace-leaf" prop=background-color
obsidian dev:mobile on                              # toggle mobile emulation
```

---

## Defuddle

Use Defuddle CLI to extract clean readable content from web pages. Prefer over WebFetch for standard web pages — it removes navigation, ads, and clutter, reducing token usage.

If not installed: `npm install -g defuddle`

Always use `--md` for markdown output:

```bash
defuddle parse <url> --md
```

Save to file:
```bash
defuddle parse <url> --md -o content.md
```

Extract specific metadata:
```bash
defuddle parse <url> -p title
defuddle parse <url> -p description
defuddle parse <url> -p domain
```

| Flag | Format |
|------|--------|
| `--md` | Markdown (default choice) |
| `--json` | JSON with both HTML and markdown |
| (none) | HTML |
| `-p <name>` | Specific metadata property |

---

## Reference Files

Load these on demand — only when working on the relevant file type.

- [`references/obsidian-markdown.md`](references/obsidian-markdown.md) — Wikilinks, embeds, callouts, properties, tags, math, Mermaid, footnotes
  - [`references/callouts.md`](references/callouts.md) — All callout types with aliases
  - [`references/embeds.md`](references/embeds.md) — Audio, video, PDF, search embeds
  - [`references/properties.md`](references/properties.md) — All property types and tag syntax
- [`references/obsidian-bases.md`](references/obsidian-bases.md) — Schema, filter syntax, formulas, view types, summaries, examples
  - [`references/functions-reference.md`](references/functions-reference.md) — Complete function reference for all Bases types
- [`references/json-canvas.md`](references/json-canvas.md) — Nodes, edges, groups, colors, layout, validation
  - [`references/canvas-examples.md`](references/canvas-examples.md) — Full canvas examples: mind maps, project boards, flowcharts
