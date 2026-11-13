# Development Guide

## 1. Environment

Install Node.js and npm. Install MongoDB locally or use an isolated development database. Never use production records for local testing.

## 2. Install

```bash
npm ci
```

The current package manifest contains a local self-reference dependency and does not clearly declare a complete React build/test toolchain for the tracked source. If installation or frontend commands fail, inspect `package.json` and `package-lock.json` carefully. Do not treat the current package metadata as a fully configured client application.

## 3. Start the API

Ensure MongoDB is running, then:

```bash
node server.js
```

The current connection URI is hard-coded to `mongodb://localhost:27017/mern-app`. The API listens on port 8000.

## 4. Frontend workflow

The tracked frontend uses the familiar React entry structure, but the root package does not clearly define the scripts and dependencies required to build it. Before adding frontend CI, establish a supported build system and explicit scripts. Keep that as a separate implementation change so documentation does not pretend the toolchain already exists.

## 5. API development

When adding or changing an endpoint:
1. Define the HTTP method and path.
2. Define the request and response schema.
3. Validate URL parameters and request body.
4. Handle not-found conditions.
5. Handle database errors without leaking internals.
6. Add tests for normal and failure paths.
7. Update `docs/API.md`.

Avoid adding business logic directly to the route callback if it becomes difficult to test. Extract model and service behavior as the API grows.

## 6. Frontend development

For a new component:
- Keep rendering and API state handling understandable.
- Show loading, empty, error, and success states.
- Avoid hard-coded server secrets.
- Use a configurable API base URL.
- Keep forms accessible and validate user input.
- Test the component with representative props and responses.

## 7. Current CI

The CI workflow checks `server.js` syntax with Node.js. Run the same check locally:

```bash
node --check server.js
```

This check does not connect to MongoDB or test route behavior.

## 8. Recommended tests

Add automated tests for creating a todo, listing records, updating an existing ID, updating a missing ID, deleting an existing record, deleting a missing record, invalid IDs, malformed JSON, missing title, database failure, and large request bodies.

Use a temporary database and clean up records after each test.
