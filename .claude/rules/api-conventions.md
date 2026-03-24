# API Conventions

## REST API Standards

### HTTP Methods

- `GET`: Retrieve resources
- `POST`: Create resources
- `PUT`: Update entire resource
- `PATCH`: Partial update
- `DELETE`: Remove resource

### URL Structure

- Use nouns, not verbs: `/users` not `/getUsers`
- Plural for collections: `/users` not `/user`
- Use kebab-case: `/user-profiles`
- Nest for relationships: `/users/{id}/orders`

### Response Format

```json
{
  "data": { },
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 100
  },
  "error": null
}
```

### Status Codes

- `200`: Success
- `201`: Created
- `400`: Bad Request
- `401`: Unauthorized
- `403`: Forbidden
- `404`: Not Found
- `500`: Server Error

### Error Format

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input",
    "details": [
      { "field": "email", "message": "Must be valid email" }
    ]
  }
}
```

## Versioning

- Include version in URL: `/api/v1/users`
- Or use header: `Accept: application/vnd.api.v1+json`
