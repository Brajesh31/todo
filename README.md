# Todo Application

A small full-stack todo-list prototype. The repository contains a React client in `src/` and an Express/Mongoose API in `server.js`. The API stores todo documents in MongoDB and exposes create, list, update, and delete operations.

## Requirements

- Node.js and npm
- MongoDB running locally or a configured MongoDB deployment

## Setup

Install dependencies from the repository root:

```bash
npm ci
```

Start the API:

```bash
node server.js
```

The API listens on port 8000 and currently connects to `mongodb://localhost:27017/mern-app`. The connection string and port should be moved to environment variables before deployment.

## API

| Method | Path | Behavior |
|---|---|---|
| GET | `/todos` | Return all todo documents |
| POST | `/todos` | Create a todo using `title` and optional `description` |
| PUT | `/todos/:id` | Update a todo by MongoDB document ID |
| DELETE | `/todos/:id` | Delete a todo by MongoDB document ID |

## Current limitations

This is a prototype, not a multi-user task service. The API has no authentication or per-user authorization, CORS is open, input validation is minimal, and the delete handler does not distinguish a missing record. The root package manifest also has a placeholder failing test script and a local self-reference dependency. Verify dependency installation and configure a proper client build/test toolchain before relying on CI or deployment.

See [architecture](docs/ARCHITECTURE.md), [API reference](docs/API.md), [development](docs/DEVELOPMENT.md), and [deployment](docs/DEPLOYMENT.md).

Last updated: 2026-11-13 19:34 UTC
