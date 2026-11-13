# Deployment

Before deployment:
- Move MongoDB URI and port configuration into environment variables.
- Restrict CORS to the exact trusted frontend origins.
- Add authentication and per-user authorization before storing non-public tasks.
- Validate title, description, and MongoDB IDs at the API boundary.
- Return 404 for missing records and use consistent error response shapes.
- Add rate limits, request logging, monitoring, and database backups.
- Serve the built React client over HTTPS and configure the API base URL for the target environment.

The current API listens on port 8000 and uses a local MongoDB URI. These defaults are for local development only.
