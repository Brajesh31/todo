# Deployment and Operational Readiness

## 1. Current state

The API is a local development prototype. It binds to port 8000 and connects to a local MongoDB instance. CORS is currently broad. There is no authentication, authorization, rate limiting, health check, or production configuration validation.

Do not deploy this service with private or sensitive task data.

## 2. Configuration

Move the following into validated environment variables:
- MongoDB connection URI.
- Server port.
- Allowed frontend origins.
- Logging level and environment mode.

Do not commit secrets or put database credentials into browser code.

## 3. Network and CORS

Replace unrestricted CORS with an allowlist of exact trusted frontend origins. Configure the reverse proxy to expose only the required API routes. Use HTTPS and apply request size limits.

## 4. Authentication and authorization

Before public deployment, implement authentication and a user ownership model. Every task must be associated with an owner, and every read/update/delete must include that owner in the database query. Test that one account cannot access another account's task by guessing an ID.

## 5. Database operations

Use a dedicated database user with least-privilege access. Configure backups and verify restore procedures. Monitor database connectivity and query latency. Add indexes only after examining query patterns.

## 6. Process lifecycle

The service should fail startup if required configuration is missing or the database cannot be reached. Add graceful shutdown handling so in-flight requests and database connections are closed cleanly.

## 7. Observability

Use structured logs with request IDs. Redact personal task content and secrets. Add error monitoring, uptime checks, and alerts for repeated database failures.

## 8. Release verification

After deployment, verify API health, create/list/update/delete flows, CORS behavior, error handling, database persistence after restart, and access controls. Until authentication exists, test only with disposable non-sensitive data.
