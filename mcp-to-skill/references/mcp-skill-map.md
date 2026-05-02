# MCP-to-Skill Conversion Map

Pre-built skill wrappers for currently connected MCPs. Each section below is a ready-to-use SKILL.md body that wraps an MCP's tools as an on-demand skill.

---

## 1. Firecrawl (Web Scraping & Crawling)

**MCP Config:** `npx -y firecrawl-mcp`
**Category:** Sometimes needed — convert to skill

```yaml
name: firecrawl-tools
description: "Scrape, crawl, and extract structured data from websites using Firecrawl.
Use this skill any time the user wants to scrape a webpage, crawl a site, extract
content from URLs, or convert web pages to markdown. Trigger especially when the user
mentions 'scrape', 'crawl website', 'extract from URL', 'web content', or provides a
URL to read. Do NOT trigger for browser automation (use browser-agent-automation) or
search queries without a specific URL (use brave-search-tools)."
```

**Key Tools:**
| Tool | Purpose |
|------|---------|
| `firecrawl_scrape` | Scrape a single URL to markdown/structured data |
| `firecrawl_crawl` | Crawl an entire site following links |
| `firecrawl_map` | Get a sitemap of all URLs on a domain |
| `firecrawl_extract` | Extract structured data using a schema |

**Typical Workflow:**
1. `firecrawl_map` to discover pages on the domain
2. `firecrawl_scrape` on specific pages of interest
3. `firecrawl_extract` if structured data is needed

---

## 2. Brave Search (Web Search)

**MCP Config:** `npx -y @brave/brave-search-mcp-server`
**Category:** Sometimes needed — convert to skill

```yaml
name: brave-search-tools
description: "Search the web using Brave Search for real-time information, research, and
fact-checking. Use this skill any time the user asks to search the web, find current
information, look up recent events, or research a topic online. Trigger especially when
the user says 'search for', 'look up', 'find online', 'what is the latest', or needs
information beyond the training cutoff. Do NOT trigger for searching within the codebase
(use Grep/Glob) or for scraping specific URLs (use firecrawl-tools)."
```

**Key Tools:**
| Tool | Purpose |
|------|---------|
| `brave_web_search` | Search the web and return results with snippets |
| `brave_local_search` | Search for local businesses and places |

---

## 3. Figma (Design Extraction)

**MCP Config:** `npx -y @anthropic/figma-mcp`
**Category:** Sometimes needed — convert to skill

```yaml
name: figma-tools
description: "Extract design data from Figma files including layouts, styles, components,
and design tokens. Use this skill any time the user wants to implement a Figma design,
extract colors/typography/spacing from Figma, or reference a Figma file for code
generation. Trigger especially when the user mentions 'Figma', shares a Figma URL,
wants to convert designs to code, or asks about design tokens from their design system.
Do NOT trigger for creating designs from scratch without Figma (use frontend-design) or
for general UI implementation without a Figma reference (use ui-to-code)."
```

**Key Tools:**
| Tool | Purpose |
|------|---------|
| `figma_get_file` | Get full file structure and metadata |
| `figma_get_node` | Get a specific frame/component by node ID |
| `figma_get_styles` | Extract color, text, and effect styles |
| `figma_get_components` | List all components in a file |
| `figma_get_images` | Export nodes as PNG/SVG/PDF |

**Typical Workflow:**
1. `figma_get_file` to understand overall structure
2. `figma_get_node` for the specific frame to implement
3. `figma_get_styles` for design tokens
4. `figma_get_images` to export assets

---

## 4. NanoBanana (AI Image Generation)

**MCP Config:** `npx -y nanobanana-mcp`
**Category:** Rarely needed — convert to skill

```yaml
name: nanobanana-tools
description: "Generate AI images using Google Gemini via NanoBanana. Use this skill any
time the user wants to generate images, create product photos, mockups, illustrations,
or AI art. Trigger especially when the user says 'generate an image', 'create a photo',
'make a mockup', or wants visual content created by AI. Do NOT trigger for editing
existing images (use image editing tools) or for screenshot capture (use browser tools)."
```

**Key Tools:**
| Tool | Purpose |
|------|---------|
| `generate_image` | Generate an image from a text prompt |

---

## 5. Sequential Thinking (Reasoning Chains)

**MCP Config:** `npx -y @modelcontextprotocol/server-sequential-thinking`
**Category:** Sometimes needed — convert to skill

```yaml
name: sequential-thinking-tools
description: "Structured step-by-step reasoning for complex problems using sequential
thinking chains. Use this skill any time the user presents a complex problem requiring
multi-step reasoning, mathematical proofs, logical deductions, or decision trees. Trigger
especially when the user says 'think through this step by step', 'reason about',
'analyze this complex problem', or presents a problem with multiple interdependent
variables. Do NOT trigger for simple questions or straightforward coding tasks."
```

**Key Tools:**
| Tool | Purpose |
|------|---------|
| `sequentialthinking` | Create a step-by-step reasoning chain |

---

## 6. Sentry (Error Monitoring)

**MCP Plugin:** `sentry`
**Category:** Sometimes needed — convert to skill

```yaml
name: sentry-tools
description: "Monitor, investigate, and manage production errors using Sentry. Use this
skill any time the user wants to check for production errors, investigate an issue from
Sentry, analyze error trends, or manage Sentry projects/teams. Trigger especially when
the user mentions 'Sentry', 'production error', 'error monitoring', 'issue tracking',
or wants to debug a problem that's happening in production. Do NOT trigger for local
debugging (use systematic-debugging) or for setting up monitoring infrastructure
(use observability-incident)."
```

**Key Tools:**
| Tool | Purpose |
|------|---------|
| `search_issues` | Search for Sentry issues by query |
| `get_issue_details` | Get full details of a specific issue |
| `search_events` | Search error events with filters |
| `get_trace_details` | View distributed trace data |
| `analyze_issue_with_seer` | AI-powered root cause analysis |
| `create_project` / `create_team` | Manage Sentry organization |

---

## 7. GitHub (Repository Management)

**MCP Plugin:** `github`
**Category:** Sometimes needed — convert to skill

```yaml
name: github-tools
description: "Manage GitHub repositories, issues, pull requests, and code search via the
GitHub API. Use this skill any time the user wants to create/review PRs, manage issues,
search code across GitHub, fork repos, or interact with GitHub beyond local git. Trigger
especially when the user mentions 'GitHub', 'pull request', 'issue', 'PR review', or
wants to interact with remote repository features. Do NOT trigger for local git
operations (use git-workflow) or for CI/CD pipeline setup (use admin-ai-ops)."
```

**Key Tools (20+):**
| Tool | Purpose |
|------|---------|
| `create_pull_request` | Create a PR |
| `get_pull_request` | Get PR details |
| `list_issues` / `create_issue` | Manage issues |
| `search_code` | Search code across GitHub |
| `create_or_update_file` | Edit files via API |
| `push_files` | Push multiple files |
| `create_branch` | Create a branch remotely |

---

## 8. Supabase (Database & Backend)

**MCP Plugin:** `supabase`
**Category:** Sometimes needed — convert to skill

```yaml
name: supabase-tools
description: "Manage Supabase projects including database operations, migrations, edge
functions, and infrastructure. Use this skill any time the user wants to run SQL queries,
create database migrations, deploy edge functions, manage Supabase projects, or check
database state. Trigger especially when the user mentions 'Supabase', 'database',
'migration', 'edge function', 'SQL', or is working with a Supabase-backed project.
Do NOT trigger for general SQL/database design patterns (use database-patterns) or for
non-Supabase backend services (use backend-api-contracts)."
```

**Key Tools (20+):**
| Tool | Purpose |
|------|---------|
| `execute_sql` | Run SQL queries |
| `apply_migration` | Create and apply migrations |
| `list_tables` | View database schema |
| `deploy_edge_function` | Deploy serverless functions |
| `list_projects` | View all projects |
| `get_logs` | Read application logs |

---

## 9. Gmail (Email)

**MCP Plugin:** `Gmail`
**Category:** Rarely needed — convert to skill

```yaml
name: gmail-tools
description: "Read, search, and draft emails in Gmail. Use this skill any time the user
wants to check their email, search for specific messages, draft replies, or manage their
inbox. Trigger especially when the user mentions 'email', 'Gmail', 'inbox', 'send a
message', 'draft an email', or wants to find information from their email. Do NOT
trigger for Slack messages or other messaging platforms."
```

**Key Tools:**
| Tool | Purpose |
|------|---------|
| `gmail_search_messages` | Search emails by query |
| `gmail_read_message` | Read a specific email |
| `gmail_read_thread` | Read an email thread |
| `gmail_create_draft` | Draft a new email |
| `gmail_list_labels` | List email labels/folders |

---

## 10. Google Calendar (Scheduling)

**MCP Plugin:** `Google Calendar`
**Category:** Rarely needed — convert to skill

```yaml
name: gcal-tools
description: "Manage Google Calendar events, find free time, and schedule meetings. Use
this skill any time the user wants to check their schedule, create events, find available
meeting times, or manage their calendar. Trigger especially when the user mentions
'calendar', 'schedule', 'meeting', 'free time', 'book a time', or asks about upcoming
events. Do NOT trigger for project timeline planning (use feature-planning) or task
management (use project-workflow)."
```

**Key Tools:**
| Tool | Purpose |
|------|---------|
| `gcal_list_events` | List upcoming events |
| `gcal_create_event` | Create a new event |
| `gcal_find_free_time` | Find available time slots |
| `gcal_find_meeting_times` | Find mutual free time |
| `gcal_update_event` | Modify an existing event |
| `gcal_delete_event` | Remove an event |

---

## MCPs to Keep As-Is (Always Needed)

These MCPs have low tool counts and are used nearly every session — the overhead is minimal and the benefit of always-on availability outweighs conversion.

| MCP | Tools | Reason to Keep |
|-----|-------|----------------|
| **filesystem** | ~10 | Core file operations, used every session |
| **time** | 1-2 | Minimal overhead, frequently needed |
| **playwright** | ~5 | Often needed for testing, low overhead |
| **context7** | 2 | Documentation lookup, frequently useful |
| **claude-in-chrome** | ~15 | Browser automation, often interactive |
| **browser-tools** | ~10 | Complements chrome automation |
