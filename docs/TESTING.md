# Testing Strategy

## 1. Current validation

The current CI workflow runs `node --check server.js`. This confirms the backend entrypoint parses, but it does not execute the server or test database interactions. There is no comprehensive integration suite currently documented.

## 2. Syntax check

```bash
node --check server.js
```

## 3. API test matrix

| Feature | Scenario | Expected result |
|---|---|---|
| GET | Empty database | Empty JSON array |
| GET | Several records | All current records returned |
| POST | Valid title and description | 201 and created document |
| POST | Missing title | Validation error |
| POST | Whitespace-only title | Should be rejected after validation hardening |
| PUT | Existing ID and valid body | Updated document |
| PUT | Unknown ID | 404 |
| PUT | Malformed ID | Client error, not unhandled 500 |
| DELETE | Existing ID | 204 |
| DELETE | Unknown ID | Should return 404 after behavior is corrected |
| Database | Connection unavailable | Clear service failure, no unhandled crash |

## 4. Integration testing

Use a disposable MongoDB database. Before each test, create known records. After each test, remove inserted records or drop the test collection. Do not connect test runs to a production database.

## 5. Frontend tests

Once the frontend package scripts and test dependencies are made explicit, test:
- Initial loading state.
- Empty task list.
- Populated task list.
- Create form validation.
- Edit form prefilled values.
- Delete confirmation and success state.
- API error messages.
- Network failure handling.

## 6. Security tests

Before any public deployment, test that unauthenticated access is rejected after authentication is added, users can access only their own tasks, CORS rejects unknown origins, large request bodies are rejected, and logs do not include secrets or private task content.

## 7. CI expansion

A future pipeline should install dependencies, run formatting/lint checks, build the frontend, run backend unit tests, start an isolated MongoDB service for integration tests, and publish test results. Do not claim these checks exist until they are added and observed passing.
