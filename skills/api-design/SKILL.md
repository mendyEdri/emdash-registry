---
name: api-design
description: Design RESTful and GraphQL APIs following best practices. Use when planning new API endpoints, refactoring existing APIs, or creating API specifications.
user_invocable: true
tools: []
tags: [api, rest, graphql, backend]
---

# API Design

This skill guides the design of clean, consistent, and developer-friendly APIs.

## Design Principles

1. **Resource-Oriented**: Design around resources (nouns), not actions (verbs)
2. **Consistency**: Use consistent naming, response formats, and error handling
3. **Versioning**: Plan for API evolution from the start
4. **Documentation**: APIs should be self-documenting where possible

## RESTful API Guidelines

### URL Structure
- Use plural nouns: `/users`, `/orders`, `/products`
- Nest for relationships: `/users/{id}/orders`
- Use query params for filtering: `/products?category=electronics&sort=price`
- Keep URLs lowercase with hyphens: `/order-items` not `/orderItems`

### HTTP Methods
- `GET`: Retrieve resources (idempotent)
- `POST`: Create new resources
- `PUT`: Full update of resource
- `PATCH`: Partial update of resource
- `DELETE`: Remove resource

### Response Codes
- `200`: Success
- `201`: Created
- `204`: No content (successful delete)
- `400`: Bad request (client error)
- `401`: Unauthorized
- `403`: Forbidden
- `404`: Not found
- `422`: Validation error
- `500`: Server error

### Response Format
```json
{
  "data": { ... },
  "meta": {
    "page": 1,
    "total": 100
  }
}
```

### Error Format
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Email is required",
    "details": [
      { "field": "email", "message": "Required field" }
    ]
  }
}
```

## GraphQL Guidelines

- Use clear, descriptive type names
- Implement proper pagination (cursor-based)
- Design mutations to return affected data
- Use input types for complex arguments
- Implement proper error handling

## Security Considerations

- Always use HTTPS
- Implement rate limiting
- Validate all inputs
- Use proper authentication (OAuth2, JWT)
- Don't expose internal IDs unnecessarily
