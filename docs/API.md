# API reference

Base URL for local development: `http://localhost:8000`.

| Method | Path | Request | Success |
|---|---|---|---|
| GET | `/todos` | None | JSON array of todo documents |
| POST | `/todos` | JSON `{ "title": "…", "description": "…" }` | 201 and created document |
| PUT | `/todos/:id` | JSON `{ "title": "…", "description": "…" }` | Updated document or 404 |
| DELETE | `/todos/:id` | None | 204 |

Errors are currently returned as JSON with a `message` field and HTTP 500 in most exception cases. The delete route currently returns 204 even when no document matched. There is no authentication, pagination, filtering, or ownership boundary.
