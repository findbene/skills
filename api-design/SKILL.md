---
name: api-design
description: 'REST and GraphQL API design specialist for endpoint structure, versioning, status codes, authentication, pagination, error handling, and O. Triggers: "use api-design", "build API design", "api design.'
allowed-tools: Glob, Grep, Read
---

# API Design Best Practices

Guidelines for building robust, developer-friendly APIs.

## REST Endpoint Design

### URL Structure
```
GET    /api/v1/users          # List users
GET    /api/v1/users/:id      # Get user
POST   /api/v1/users          # Create user
PUT    /api/v1/users/:id      # Replace user
PATCH  /api/v1/users/:id      # Update user
DELETE /api/v1/users/:id      # Delete user

# Nested resources
GET    /api/v1/users/:id/posts
POST   /api/v1/users/:id/posts
```

### Naming Conventions
- Use nouns, not verbs: `/users` not `/getUsers` — the HTTP method already conveys the action
- Plural for collections: `/users` not `/user`
- Lowercase with hyphens: `/user-profiles`
- No trailing slashes: `/users` not `/users/`

## HTTP Status Codes

| Code | Meaning | Use Case |
|------|---------|----------|
| 200 | OK | Successful GET/PUT/PATCH |
| 201 | Created | Successful POST |
| 204 | No Content | Successful DELETE |
| 400 | Bad Request | Invalid input |
| 401 | Unauthorized | Missing/invalid auth |
| 403 | Forbidden | Valid auth, no permission |
| 404 | Not Found | Resource doesn't exist |
| 409 | Conflict | Duplicate/conflict |
| 422 | Unprocessable | Validation failed |
| 429 | Too Many Requests | Rate limited |
| 500 | Server Error | Unexpected error |

## Error Response Format

Consistent error formats let API consumers write reliable error handling code:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      { "field": "email", "message": "Must be a valid email address" }
    ]
  }
}
```

## Pagination

```typescript
// Offset-based (simple, good for small datasets)
GET /api/v1/users?page=2&limit=20

// Response
{
  "data": [...],
  "pagination": {
    "page": 2,
    "limit": 20,
    "total": 150,
    "totalPages": 8,
    "hasNext": true,
    "hasPrev": true
  }
}

// Cursor-based (better for large or frequently changing datasets)
GET /api/v1/users?cursor=abc123&limit=20
```

Cursor-based pagination avoids the "skipped/duplicated items" problem that offset-based pagination has when data changes between requests.

## Filtering & Sorting

```
GET /api/v1/users?status=active&role=admin
GET /api/v1/users?sort=createdAt:desc
GET /api/v1/users?fields=id,name,email
GET /api/v1/users?search=john
```

## Authentication

```typescript
// Bearer token
Authorization: Bearer <token>

// API key
X-API-Key: <key>
```

## Versioning

```typescript
// URL versioning (recommended — explicit and easy to route)
/api/v1/users
/api/v2/users

// Header versioning
Accept: application/vnd.api+json;version=1
```

## Rate Limiting Headers

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1640000000
Retry-After: 60
```

## When NOT to use

- Internal-only RPC between trusted services where gRPC/typed clients beat REST
- One-off webhook receiver — match the producer's contract, don't redesign
- Pure data-layer schema design — use `database-schema-designer`
- Microservice boundary decisions — use `backend-architect`
- Frontend-only mocks with no real backend — sketch types in code, not a full API spec

## Red Flags

| Thought | Reality |
|---------|---------|
| "Verbs in URLs are fine: `/api/getUsers`" | The HTTP method is the verb; verbs in paths break REST conventions and tooling |
| "Return 200 with `{ error }` body for failures" | Breaks every client retry/error library; use proper 4xx/5xx codes |
| "No versioning yet, we'll add it later" | Breaking changes without a version path either freeze the API or break clients silently |
| "Offset pagination works fine for our 10M-row table" | Offset N scans N rows; performance dies past page 100. Use cursors. |

## Output Contract

Done when:
- Resource URLs use plural nouns, lowercase-hyphenated, no trailing slash
- HTTP methods map cleanly to GET/POST/PUT/PATCH/DELETE semantics
- Status codes used per the table; no 200-with-error-body patterns
- Error response envelope is consistent across endpoints with `code` + `message` + optional `details`
- Pagination strategy chosen (offset vs cursor) and documented
- Versioning strategy declared (URL or header) and applied
- OpenAPI / spec file generated or updated to match

## Examples

### Example 1 — Design CRUD endpoints for a new "projects" resource
- Input: "Add a projects API for our app"
- Action: Define `GET/POST /api/v1/projects`, `GET/PATCH/DELETE /api/v1/projects/:id`, nested `GET /api/v1/projects/:id/members`; cursor pagination for list; error envelope; Bearer auth; OpenAPI stub generated
- Output: Endpoint table, request/response Zod-style schemas, status code matrix, OpenAPI 3 fragment

### Example 2 — Audit an existing API for breaking-change risk
- Input: "We want to add a required field to /users — what's the right path?"
- Action: Identify breaking-change rule, recommend `/api/v2/users` with new required field OR feature flag via header; document deprecation timeline for v1
- Output: Migration plan with timeline, header-version fallback, OpenAPI v1 + v2 split, deprecation `Sunset` header wired
