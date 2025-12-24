# API Standards Template

Use this template for `02-standards/api-standards.md`.

---

```markdown
# API Standards

> **Version:** 1.0.0
> **Status:** Draft | Review | Approved
> **Last Updated:** YYMMDD

---

## 1. Overview

This document defines REST API design standards for consistency across all endpoints.

---

## 2. URL Structure

### Base URL

```
Production:  https://api.example.com/v1
Staging:     https://api-staging.example.com/v1
Development: http://localhost:3000/api/v1
```

### Resource Naming

| Pattern | Example | Notes |
|---------|---------|-------|
| Plural nouns | `/users`, `/orders` | NOT `/user`, `/order` |
| Kebab-case | `/user-profiles` | NOT `/userProfiles` |
| Hierarchical | `/users/{id}/orders` | Nested resources |
| Query for filtering | `/users?status=active` | NOT `/users/active` |

### URL Conventions

```
GET    /users              # List users
GET    /users/{id}         # Get single user
POST   /users              # Create user
PUT    /users/{id}         # Full update
PATCH  /users/{id}         # Partial update
DELETE /users/{id}         # Delete user

# Nested resources
GET    /users/{id}/orders  # User's orders
POST   /users/{id}/orders  # Create order for user

# Actions (when CRUD doesn't fit)
POST   /users/{id}/activate
POST   /orders/{id}/cancel
```

---

## 3. Request Format

### Headers (Required)

```http
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token>
X-Request-ID: <uuid>
```

### Request Body

```json
{
  "email": "user@example.com",
  "password": "secure123",
  "profile": {
    "firstName": "John",
    "lastName": "Doe"
  }
}
```

**Rules:**
- Use camelCase for field names
- Dates in ISO 8601: `2024-12-24T10:30:00Z`
- UUIDs for IDs (not auto-increment)
- Avoid nested objects beyond 2 levels

---

## 4. Response Format

### Standard Response Wrapper

```json
{
  "success": true,
  "data": { ... },
  "meta": {
    "requestId": "uuid",
    "timestamp": "2024-12-24T10:30:00Z"
  }
}
```

### Single Resource (200 OK)

```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "email": "user@example.com",
    "createdAt": "2024-12-24T10:30:00Z"
  }
}
```

### Collection (200 OK)

```json
{
  "success": true,
  "data": [
    { "id": "uuid1", "email": "user1@example.com" },
    { "id": "uuid2", "email": "user2@example.com" }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 100,
    "totalPages": 5,
    "hasNext": true,
    "hasPrev": false
  }
}
```

### Created (201 Created)

```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "email": "user@example.com",
    "createdAt": "2024-12-24T10:30:00Z"
  }
}
```

### No Content (204 No Content)

- Used for DELETE success
- Empty response body

---

## 5. Error Format

### Error Response Structure

```json
{
  "success": false,
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User with ID 'uuid' not found",
    "details": [
      {
        "field": "userId",
        "message": "Invalid user ID format"
      }
    ]
  },
  "meta": {
    "requestId": "uuid",
    "timestamp": "2024-12-24T10:30:00Z"
  }
}
```

### HTTP Status Codes

| Code | Meaning | When to Use |
|------|---------|-------------|
| 200 | OK | Successful GET, PUT, PATCH |
| 201 | Created | Successful POST (resource created) |
| 204 | No Content | Successful DELETE |
| 400 | Bad Request | Invalid input, validation failed |
| 401 | Unauthorized | Missing or invalid auth token |
| 403 | Forbidden | Valid auth but no permission |
| 404 | Not Found | Resource doesn't exist |
| 409 | Conflict | Duplicate resource, state conflict |
| 422 | Unprocessable Entity | Semantic validation failed |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Server-side error |
| 503 | Service Unavailable | Maintenance, overloaded |

### Error Codes Convention

```
[DOMAIN]_[ACTION]_[REASON]

Examples:
- USER_NOT_FOUND
- AUTH_TOKEN_EXPIRED
- ORDER_ALREADY_CANCELLED
- PAYMENT_INSUFFICIENT_FUNDS
- VALIDATION_EMAIL_INVALID
```

---

## 6. Pagination

### Query Parameters

```
GET /users?page=1&limit=20&sort=createdAt&order=desc
```

| Parameter | Type | Default | Max | Description |
|-----------|------|---------|-----|-------------|
| page | int | 1 | - | Page number (1-indexed) |
| limit | int | 20 | 100 | Items per page |
| sort | string | createdAt | - | Sort field |
| order | string | desc | - | asc or desc |

### Response

```json
{
  "data": [...],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 150,
    "totalPages": 8,
    "hasNext": true,
    "hasPrev": false
  }
}
```

---

## 7. Filtering & Search

### Query Parameters

```
GET /users?status=active&role=admin&search=john
GET /orders?createdAt[gte]=2024-01-01&createdAt[lte]=2024-12-31
GET /products?price[min]=10&price[max]=100
```

### Filter Operators

| Operator | Example | Meaning |
|----------|---------|---------|
| equals | `status=active` | Exact match |
| in | `status[in]=active,pending` | In list |
| gte | `createdAt[gte]=2024-01-01` | Greater than or equal |
| lte | `createdAt[lte]=2024-12-31` | Less than or equal |
| search | `search=keyword` | Full-text search |

---

## 8. Versioning

### URL-based (Recommended)

```
/v1/users
/v2/users
```

### Version Lifecycle

| Version | Status | End of Life |
|---------|--------|-------------|
| v1 | Active | TBD |
| v2 | Beta | - |

### Breaking Changes

When incrementing version:
- Removing fields
- Changing field types
- Changing error codes
- Changing authentication

Non-breaking (no version bump):
- Adding new fields
- Adding new endpoints
- Adding new query parameters

---

## 9. Authentication

### Bearer Token

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### Token Refresh

```
POST /auth/refresh
{
  "refreshToken": "..."
}
```

### API Keys (Service-to-Service)

```http
X-API-Key: sk_live_xxxxx
```

---

## 10. Rate Limiting

### Response Headers

```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1703424000
```

### Rate Limit Tiers

| Tier | Requests/min | Burst |
|------|--------------|-------|
| Anonymous | 20 | 5 |
| Authenticated | 100 | 20 |
| Premium | 500 | 100 |

### 429 Response

```json
{
  "success": false,
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Too many requests. Retry after 60 seconds.",
    "retryAfter": 60
  }
}
```

---

## 11. Open Questions

| # | Question | Impact | Status |
|---|----------|--------|--------|
| 1 | [API design decision] | [Impact] | Open |

---

## 12. Changelog

### YYMMDD - v1.0.0 - Initial Draft
- Defined URL structure
- Set response format
- Established error codes
```

---

## Validation Rules

- [ ] Version header present
- [ ] URL structure documented
- [ ] Response format standardized
- [ ] Error codes defined
- [ ] Pagination format defined
- [ ] Rate limiting documented
