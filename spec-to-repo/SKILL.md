---
name: spec-to-repo
description: "Use when the user says 'build me an app', 'create a project from this spec', 'scaffold a new repo', 'generate a starter', 'turn this idea into code', 'bootstrap a project', 'I have requirements and n."
allowed-tools: Bash, Glob, Grep, Read
---

# Spec to Repo

Turn a natural-language project specification into a complete, runnable starter repository. Not a template filler — a spec interpreter that generates real, working code for any stack.

## When to Use

- User provides a text description of an app and wants code
- User has a PRD, requirements doc, or feature list and needs a codebase
- User says "build me an app that...", "scaffold this", "bootstrap a project"
- User wants a working starter repo, not just a file tree

**Not this skill** when the user wants a SaaS app with Stripe + Auth specifically — use `product-team/saas-scaffolder` instead.

## Core Workflow

### Phase 1 — Parse & Interpret

Read the spec. Extract these fields silently:

| Field | Source | Required |
|-------|--------|----------|
| App name | Explicit or infer from description | yes |
| Description | First sentence of spec | yes |
| Features | Bullet points or sentences describing behavior | yes |
| Tech stack | Explicit ("use FastAPI") or infer from context | yes |
| Auth | "login", "users", "accounts", "roles" | if mentioned |
| Database | "store", "save", "persist", "records", "schema" | if mentioned |
| API surface | "endpoint", "API", "REST", "GraphQL" | if mentioned |
| Deploy target | "Vercel", "Docker", "AWS", "Railway" | if mentioned |

**Stack inference rules** (when user doesn't specify):

| Signal | Inferred stack |
|--------|---------------|
| "web app", "dashboard", "SaaS" | Next.js + TypeScript |
| "API", "backend", "microservice" | FastAPI (Python) or Express (Node) |
| "mobile app" | Flutter or React Native |
| "CLI tool" | Go or Python |
| "data pipeline" | Python |
| "high performance", "systems" | Rust or Go |

After parsing, present a structured interpretation back to the user:

```
## Spec Interpretation

**App:** [name]
**Stack:** [framework + language]
**Features:**
1. [feature]
2. [feature]

**Database:** [yes/no — engine]
**Auth:** [yes/no — method]
**Deploy:** [target]

Does this match your intent? Any corrections before I generate?
```

Flag ambiguities. Ask **at most 3** clarifying questions. If the user says "just build it", proceed with best-guess defaults.

### Phase 2 — Architecture

Design the project before writing any files:

1. **Select template** — Match to a stack template from `references/stack-templates.md` → verify: step output matches expected outcome
2. **Define file tree** — List every file that will be created → verify: output exists + parses without error
3. **Map features to files** — Each feature gets at minimum one file/component → verify: step output matches expected outcome
4. **Design database schema** — If applicable, define tables/collections with fields and types → verify: step output matches expected outcome
5. **Identify dependencies** — List every package with version constraints → verify: step output matches expected outcome
6. **Plan API routes** — If applicable, list every endpoint with method, path, request/response shape → verify: step output matches expected outcome

Present the file tree to the user before generating:

```
project-name/
├── README.md
├── .env.example
├── .gitignore
├── .github/workflows/ci.yml
├── package.json / requirements.txt / go.mod
├── src/
│   ├── ...
├── tests/
│   ├── ...
└── ...
```

### Phase 3 — Generate

Write every file. Rules:

- **Real code, not stubs.** Every function has a real implementation. No `// TODO: implement` or `pass` placeholders.
- **Syntactically valid.** Every file must parse without errors in its language.
- **Imports match dependencies.** Every import must correspond to a package in the manifest (package.json, requirements.txt, go.mod, etc.).
- **Types included.** TypeScript projects use types. Python projects use type hints. Go projects use typed structs.
- **Environment variables.** Generate `.env.example` with every required variable, commented with purpose.
- **README.md.** Include: project description, prerequisites, setup steps (clone, install, configure env, run), and available scripts/commands.
- **CI config.** Generate `.github/workflows/ci.yml` with: install, lint (if linter in deps), test, build.
- **.gitignore.** Stack-appropriate ignores (node_modules, __pycache__, .env, build artifacts).

**File generation order:**
1. Manifest (package.json / requirements.txt / go.mod) → verify: step output matches expected outcome
2. Config files (.env.example, .gitignore, CI) → verify: step output matches expected outcome
3. Database schema / migrations → verify: step output matches expected outcome
4. Core business logic → verify: step output matches expected outcome
5. API routes / endpoints → verify: step output matches expected outcome
6. UI components (if applicable) → verify: step output matches expected outcome
7. Tests
8. README.md

### Phase 4 — Validate

After generation, run through this checklist:

- [ ] Every imported package exists in the manifest
- [ ] Every file referenced by an import exists in the tree
- [ ] `.env.example` lists every env var used in code
- [ ] `.gitignore` covers build artifacts and secrets
- [ ] README has setup instructions that actually work
- [ ] No hardcoded secrets, API keys, or passwords
- [ ] At least one test file exists
- [ ] Build/start command is documented and would work

Run `scripts/validate_project.py` against the generated directory to catch common issues.

## Examples

### Example 1: Task Management API

**Input spec:**
> "Build me a task management API. Users can create, list, update, and delete tasks. Tasks have a title, description, status (todo/in-progress/done), and due date. Use FastAPI with SQLite. Add basic auth with API keys."

**Output file tree:**
```
task-api/
├── README.md
├── .env.example              # API_KEY, DATABASE_URL
├── .gitignore
├── .github/workflows/ci.yml
├── requirements.txt          # fastapi, uvicorn, sqlalchemy, pytest
├── main.py                   # FastAPI app, CORS, lifespan
├── models.py                 # SQLAlchemy Task model
├── schemas.py                # Pydantic request/response schemas
├── database.py               # SQLite engine + session
├── auth.py                   # API key middleware
├── routers/
│   └── tasks.py              # CRUD endpoints
└── tests/
    └── test_tasks.py         # Smoke tests for each endpoint
```

### Example 2: Recipe Sharing Web App

**Input spec:**
> "I want a recipe sharing website. Users sign up, post recipes with ingredients and steps, browse other recipes, and save favorites. Use Next.js with Tailwind. Store data in PostgreSQL."

**Output file tree:**
```
recipe-share/
├── README.md
├── .env.example              # DATABASE_URL, NEXTAUTH_SECRET, NEXTAUTH_URL
├── .gitignore
├── .github/workflows/ci.yml
├── package.json              # next, react, tailwindcss, prisma, next-auth
├── tailwind.config.ts
├── tsconfig.json
├── next.config.ts
├── prisma/
│   └── schema.prisma         # User, Recipe, Ingredient, Favorite models
├── src/
│   ├── app/
│   │   ├── layout.tsx
│   │   ├── page.tsx          # Homepage — recipe feed
│   │   ├── recipes/
│   │   │   ├── page.tsx      # Browse recipes
│   │   │   ├── [id]/page.tsx # Recipe detail
│   │   │   └── new/page.tsx  # Create recipe form
│   │   └── api/
│   │       ├── auth/[...nextauth]/route.ts
│   │       └── recipes/route.ts
│   ├── components/
│   │   ├── RecipeCard.tsx
│   │   ├── RecipeForm.tsx
│   │   └── Navbar.tsx
│   └── lib/
│       ├── prisma.ts
│       └── auth.ts
└── tests/
    └── recipes.test.ts
```

### Example 3: CLI Expense Tracker

**Input spec:**
> "Python CLI tool for tracking expenses. Commands: add, list, summary, export-csv. Store in a local SQLite file. No external API."

**Output file tree:**
```
expense-tracker/
├── README.md
├── .gitignore
├── .github/workflows/ci.yml
├── pyproject.toml
├── src/
│   └── expense_tracker/
│       ├── __init__.py
│       ├── cli.py            # argparse commands
│       ├── database.py       # SQLite operations
│       ├── models.py         # Expense dataclass
│       └── formatters.py     # Table + CSV output
└── tests/
    └── test_cli.py
```

## Anti-Patterns

| Anti-pattern | Fix |
|---|---|
| **Placeholder code** — `// TODO: implement`, `pass`, empty function bodies | Every function has a real implementation. If complex, implement a working simplified version. |
| **Stack override** — picking Next.js when the user said Flask | Always honor explicit tech preferences. Only infer when the user doesn't specify. |
| **Missing .gitignore** — committing node_modules or .env | Generate stack-appropriate .gitignore as one of the first files. |
| **Phantom imports** — importing packages not in the manifest | Cross-check every import against package.json / requirements.txt before finishing. |
| **Over-engineering MVP** — adding Redis caching, rate limiting, WebSockets to a v1 | Build the minimum that works. The user can iterate. |
| **Ignoring stated preferences** — user says "PostgreSQL" and you generate MongoDB | Parse the spec carefully. Explicit preferences are non-negotiable. |
| **Missing env vars** — code reads `process.env.X` but `.env.example` doesn't list it | Every env var used in code must appear in `.env.example` with a comment. |
| **No tests** — shipping a repo with zero test files | At minimum: one smoke test per API endpoint or one test per core function. |
| **Hallucinated APIs** — generating code that calls library methods that don't exist | Stick to well-documented, stable APIs. When unsure, use the simplest approach. |

## Validation Script

### `scripts/validate_project.py`

Checks a generated project directory for common issues:

```bash
# Validate a generated project
python3 scripts/validate_project.py /path/to/generated-project

# JSON output
python3 scripts/validate_project.py /path/to/generated-project --format json
```

Checks performed:
- README.md exists and is non-empty
- .gitignore exists
- .env.example exists (if code references env vars)
- Package manifest exists (package.json, requirements.txt, go.mod, Cargo.toml, pubspec.yaml)
- No .env file committed (secrets leak)
- At least one test file exists
- No TODO/FIXME placeholders in generated code

## Progressive Enhancement

For complex specs, generate in stages:

1. **MVP** — Core feature only, working end-to-end → verify: step output matches expected outcome
2. **Auth** — Add authentication if requested → verify: dependency resolves + import works
3. **Polish** — Error handling, validation, loading states → verify: file content matches expected shape
4. **Deploy** — Docker, CI, deploy config → verify: step output matches expected outcome

Ask the user after MVP: "Core is working. Want me to add auth/polish/deploy next, or iterate on what's here?"

## Cross-References

- Related: `product-team/saas-scaffolder` — SaaS-specific scaffolding (Next.js + Stripe + Auth)
- Related: `engineering/spec-driven-workflow` — spec-first development methodology
- Related: `engineering/database-designer` — database schema design patterns
- Related: `engineering-team/senior-fullstack` — full-stack implementation patterns

## When NOT to use

- Task is unrelated to spec to repo — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Spec To Repo needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for spec to repo
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow
