# REST API Reference

## 1. Local base URL

The current Express service listens on port 8000:

```text
http://localhost:8000
```

All routes are currently mounted directly at the root. There is no version prefix such as `/api/v1`.

## 2. Todo resource

A todo document has the following visible fields:

| Field | Type | Required | Description |
|---|---|---:|---|
| `_id` | MongoDB ObjectId | Generated | Document identifier |
| `title` | String | Yes | Todo title |
| `description` | String | No | Additional detail |

Mongoose also manages document metadata according to its default schema settings.

## 3. List todos

`GET /todos`

Returns all todo documents:

```bash
curl http://localhost:8000/todos
```

The current implementation uses `Todo.find()` with no filter, sort, projection, or pagination. Every stored document is returned.

## 4. Create todo

`POST /todos`

Request:

```bash
curl -X POST http://localhost:8000/todos \
  -H "Content-Type: application/json" \
  -d '{"title":"Review pull request","description":"Check the API changes"}'
```

The route creates a Mongoose document and returns status 201 with the saved document.

The schema requires `title`. The current route does not implement a comprehensive validation contract for empty strings, whitespace-only values, maximum lengths, or unexpected property types.

## 5. Update todo

`PUT /todos/:id`

Request:

```bash
curl -X PUT http://localhost:8000/todos/REPLACE_WITH_ID \
  -H "Content-Type: application/json" \
  -d '{"title":"Review completed","description":"Updated notes"}'
```

The controller reads the ID from the URL and calls `findByIdAndUpdate` with the supplied title and description. It returns the updated document when found, or status 404 when no matching record exists.

The current update does not explicitly enable Mongoose's update validators. Invalid IDs and invalid field values should be tested and handled before production.

## 6. Delete todo

`DELETE /todos/:id`

Request:

```bash
curl -X DELETE http://localhost:8000/todos/REPLACE_WITH_ID
```

The route calls `findByIdAndDelete` and returns status 204. The current implementation does not inspect the returned document, so it does not distinguish a deleted record from a nonexistent ID.

## 7. Current status and response behavior

| Situation | Current behavior |
|---|---|
| Successful list | 200 with array |
| Successful create | 201 with created document |
| Successful update | 200 with updated document |
| Update ID not found | 404 with message |
| Successful delete | 204 with empty body |
| Route exception | 500 with message |

The error response shape is not standardized across every branch.

## 8. Missing API capabilities

Not currently implemented or documented:
- Authentication and authorization.
- Per-user task ownership.
- Pagination and sorting.
- Search and filtering.
- Due dates, status, priority, labels, or reminders.
- Bulk operations.
- Optimistic concurrency or version conflict responses.
- OpenAPI/Swagger contract.
- Rate limiting and request quotas.

## 9. Recommended API hardening

Before expanding the API, define a stable request/response schema, validate IDs and body fields, enable update validators, distinguish missing records, standardize errors, add pagination, and introduce authentication with server-side ownership checks. Do not add frontend-only access restrictions as a substitute for API authorization.
