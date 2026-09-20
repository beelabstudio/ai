---
name: api-design
description: >
  REST API design patterns: resource naming, status codes, pagination, filtering, error
  responses, versioning, and rate limiting for production APIs. Use when designing or
  reviewing REST endpoints, resource names, status codes, pagination, or versioning.
source: https://github.com/affaan-m/ECC
---

# API Design Patterns

Conventions and best practices for designing consistent, developer-friendly REST APIs.

## When to Activate

- Designing new API endpoints
- Reviewing existing API contracts
- Adding pagination, filtering, or sorting
- Implementing error handling for APIs
- Planning an API versioning strategy
- Building public or partner-facing APIs

## Resource Design

### URL Structure

```
# Resources are nouns, plural, lowercase, kebab-case
GET    /api/v1/users
GET    /api/v1/users/:id
POST   /api/v1/users
PUT    /api/v1/users/:id
PATCH  /api/v1/users/:id
DELETE /api/v1/users/:id

# Sub-resources for relationships
GET    /api/v1/users/:id/orders
POST   /api/v1/users/:id/orders

# Actions that don't map to CRUD (use verbs sparingly)
POST   /api/v1/orders/:id/cancel
POST   /api/v1/auth/login
```

### Naming Rules

```
# GOOD
/api/v1/team-members          # kebab-case for multi-word resources
/api/v1/orders?status=active  # query params for filtering
/api/v1/users/123/orders      # nested resources for ownership

# BAD
/api/v1/getUsers              # verb in URL
/api/v1/user                  # singular (use plural)
/api/v1/team_members          # snake_case in URLs
/api/v1/users/123/getOrders   # verb in nested resource
```

## HTTP Methods and Status Codes

| Method | Idempotent | Safe | Use For |
|--------|-----------|------|---------|
| GET | Yes | Yes | Retrieve resources |
| POST | No | No | Create resources, trigger actions |
| PUT | Yes | No | Full replacement |
| PATCH | No* | No | Partial update |
| DELETE | Yes | No | Remove a resource |

\*PATCH can be made idempotent with proper implementation.

```
# Success
200 OK                    — GET, PUT, PATCH (with response body)
201 Created               — POST (include Location header)
204 No Content            — DELETE, PUT (no response body)

# Client Errors
400 Bad Request           — validation failure, malformed JSON
401 Unauthorized          — missing or invalid authentication
403 Forbidden             — authenticated but not authorized
404 Not Found             — resource doesn't exist
409 Conflict              — duplicate entry, state conflict
422 Unprocessable Entity  — semantically invalid (valid JSON, bad data)
429 Too Many Requests     — rate limit exceeded

# Server Errors
500 Internal Server Error — unexpected failure (never expose details)
502 Bad Gateway           — upstream service failed
503 Service Unavailable   — temporary overload, include Retry-After
```

## Response Format

### Success (single)

```json
{
  "data": { "id": "abc-123", "email": "alice@example.com" }
}
```

### Collection (with pagination)

```json
{
  "data": [ { "id": "abc-123", "name": "Alice" } ],
  "meta": { "total": 142, "page": 1, "per_page": 20, "total_pages": 8 },
  "links": {
    "self": "/api/v1/users?page=1&per_page=20",
    "next": "/api/v1/users?page=2&per_page=20",
    "last": "/api/v1/users?page=8&per_page=20"
  }
}
```

### Error

```json
{
  "error": {
    "code": "validation_error",
    "message": "Request validation failed",
    "details": [
      { "field": "email", "message": "Must be a valid email address", "code": "invalid_format" }
    ]
  }
}
```

## Pagination

### Offset-based (simple)

```
GET /api/v1/users?page=2&per_page=20
```

**Pros:** easy to implement, supports "jump to page N".
**Cons:** slow on large offsets, inconsistent with concurrent inserts.

### Cursor-based (scalable)

```
GET /api/v1/users?cursor=eyJpZCI6MTIzfQ&limit=20
```

```json
{ "data": [...], "meta": { "has_next": true, "next_cursor": "eyJpZCI6MTQzfQ" } }
```

**Pros:** consistent performance, stable with concurrent inserts.
**Cons:** cannot jump to an arbitrary page, cursor is opaque.

| Use Case | Pagination Type |
|----------|----------------|
| Admin dashboards, small datasets (<10K) | Offset |
| Infinite scroll, feeds, large datasets | Cursor |
| Public APIs | Cursor (default) with offset (optional) |
| Search results | Offset (users expect page numbers) |

## Filtering, Sorting, and Search

```
# Comparison operators (bracket notation)
GET /api/v1/products?price[gte]=10&price[lte]=100
GET /api/v1/orders?created_at[after]=2025-01-01

# Multiple values (comma-separated)
GET /api/v1/products?category=electronics,clothing

# Sorting (prefix - for descending)
GET /api/v1/products?sort=-created_at

# Full-text search
GET /api/v1/products?q=wireless+headphones

# Sparse fieldsets
GET /api/v1/users?fields=id,name,email
```

## Authentication and Authorization

```
# Bearer token
GET /api/v1/users
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...

# API key (server-to-server)
GET /api/v1/data
X-API-Key: sk_live_abc123
```

```typescript
// Resource-level authorization
app.get('/api/v1/orders/:id', async (req, res) => {
  const order = await Order.findById(req.params.id)
  if (!order) return res.status(404).json({ error: { code: 'not_found' } })
  if (order.userId !== req.user.id) return res.status(403).json({ error: { code: 'forbidden' } })
  return res.json({ data: order })
})
```

## Rate Limiting

```
HTTP/1.1 200 OK
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1640000000

HTTP/1.1 429 Too Many Requests
Retry-After: 60
```

| Tier | Limit | Window | Use Case |
|------|-------|--------|----------|
| Anonymous | 30/min | Per IP | Public endpoints |
| Authenticated | 100/min | Per user | Standard API access |
| Premium | 1000/min | Per API key | Paid API plans |
| Internal | 10000/min | Per service | Service-to-service |

## Versioning

### URL path versioning (recommended)

```
/api/v1/users
/api/v2/users
```

### Header versioning

```
GET /api/users
Accept: application/vnd.myapp.v2+json
```

### Strategy

1. Start with `/api/v1/` — don't version until you need to.
2. Maintain at most 2 active versions (current + previous).
3. Non-breaking changes don't need a new version (new fields, new optional params, new endpoints).
4. Breaking changes require a new version (removing/renaming fields, changing types, changing auth).
5. For public APIs, announce deprecation with a `Sunset` header and return `410 Gone` after sunset.

## API Design Checklist

- [ ] Resource URL follows naming conventions (plural, kebab-case, no verbs)
- [ ] Correct HTTP method used
- [ ] Appropriate status codes returned (not 200 for everything)
- [ ] Input validated with a schema (Zod, Pydantic, Bean Validation)
- [ ] Error responses follow a standard format with codes and messages
- [ ] Pagination implemented for list endpoints (cursor or offset)
- [ ] Authentication required (or explicitly marked public)
- [ ] Authorization checked (users can only access their own resources)
- [ ] Rate limiting configured
- [ ] Response does not leak internal details (stack traces, SQL errors)
- [ ] Consistent naming with existing endpoints
- [ ] Documented (OpenAPI/Swagger updated)
