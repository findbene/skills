---
name: slides
description: "HTML presentation creation with Chart.js, design tokens, and data visualization for strategic pitch decks and technical slide presentations. Use this skill any time HTML-based presentations need to be created, slide decks with charts need to be built, or structured visual presentations need to be generated as code. Trigger immediately on: \"slides\", \"create slides\", \"HTML presentation\", \"slide deck\", \"presentation code\", \"Chart.js slides\", \"data visualization slides\", \"slide design\", \"build a presentation\", \"create a deck\", \"visual presentation\", \"presentation HTML\", \"presentation generator\". If someone says \"create a slide deck\" or \"build an HTML presentation\" this skill MUST trigger."
argument-hint: "[topic] [slide-count]"
metadata:
  author: claudekit
  version: "1.0.0"
---

# Slides

Strategic HTML presentation design with data visualization.

<args>$ARGUMENTS</args>

## When to Use

- Marketing presentations and pitch decks
- Data-driven slides with Chart.js
- Strategic slide design with layout patterns
- Copywriting-optimized presentation content

## Subcommands

| Subcommand | Description | Reference |
|------------|-------------|-----------|
| `create` | Create strategic presentation slides | `references/create.md` |

## References (Knowledge Base)

| Topic | File |
|-------|------|
| Layout Patterns | `references/layout-patterns.md` |
| HTML Template | `references/html-template.md` |
| Copywriting Formulas | `references/copywriting-formulas.md` |
| Slide Strategies | `references/slide-strategies.md` |

## Routing

1. Parse subcommand from `$ARGUMENTS` (first word)
2. Load corresponding `references/{subcommand}.md`
3. Execute with remaining arguments
