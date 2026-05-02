---
name: api-design
description: "REST and GraphQL API design specialist for endpoint structure, versioning, status codes, authentication, pagination, error handling, and OpenAPI documentation. Use this skill any time a new API is being designed, API contracts need to be defined, REST vs GraphQL decisions need to be made, or existing APIs need to be reviewed for quality. Trigger immediately on: \"API design\", \"REST API\", \"GraphQL\", \"endpoint structure\", \"API versioning\", \"status codes\", \"API pagination\", \"API contract\", \"API documentation\", \"API review\", \"OpenAPI\", \"Swagger\", \"API schema\", \"authentication header\". If someone says \"how should I design my API?\" or \"review my API endpoints\" this skill MUST trigger."
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
