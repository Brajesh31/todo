# Todo Application, React Client and Express API

This repository contains a small full-stack todo-list prototype. The frontend is a React application, while the backend is a Node.js/Express service that stores todo documents in MongoDB through Mongoose. The API exposes create, list, update, and delete operations.

The project demonstrates a basic browser-to-API-to-database flow. It is not a production multi-user task manager. The current API has no authentication or authorization, allows cross-origin requests broadly, uses a hard-coded local MongoDB connection string, and performs only limited request validation.

## Technology stack

| Layer | Technology |
|---|---|
| Frontend | React and standard CSS |
| Backend | Node.js and Express |
| Database | MongoDB |
| ODM | Mongoose |
| Cross-origin requests | cors middleware |
| Package management | npm |

## Repository layout

```text
.
├── public/             # Browser entry HTML, icons, manifest, robots file
├── src/
│   ├── App.js          # Main React application
│   ├── Todo.js         # Todo UI component
│   ├── App.test.js     # Existing React test scaffold
│   ├── index.js        # React entrypoint
│   ├── App.css
│   └── index.css
├── server.js           # Express API and Mongoose model
├── package.json        # Root npm manifest
└── package-lock.json
```

The repository also contains a root-level `Todo.js` alongside `src/Todo.js`. Confirm which copy is intended before editing or removing either file.

## Requirements

- Node.js and npm.
- MongoDB available locally or remotely.
- A browser for the React client.
- Two terminal sessions for concurrent frontend and backend processes.

## Installation

From the repository root:

```bash
npm ci
```

The current root package manifest has incomplete and unusual dependency metadata, including a local self-reference dependency and a placeholder test command. If installation or client startup fails, inspect the package manifest and lockfile rather than assuming the frontend toolchain is complete.

## Start the API

The backend entrypoint is `server.js`. It currently connects to MongoDB at `mongodb://localhost:27017/mern-app` and listens on port 8000.

```bash
node server.js
```

The connection string and port are hard-coded in the current source. They are suitable only as local development defaults.

## Start the frontend

The tracked client source is under `src/` and resembles a Create React App structure. The current root manifest does not clearly declare the complete React scripts/dependencies required by that source. A coherent frontend toolchain must be configured before assuming `npm start` or `npm run build` will work.

If the frontend tooling is installed and configured, run its documented start command from the project directory. Keep the API on port 8000 and configure the client to use the correct API origin.

## API reference

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/todos` | Return all todo documents |
| POST | `/todos` | Create a todo |
| PUT | `/todos/:id` | Update a todo by MongoDB ID |
| DELETE | `/todos/:id` | Delete a todo by MongoDB ID |

Example create request:

```http
POST /todos
Content-Type: application/json

{
  "title": "Prepare project notes",
  "description": "Review the API and update the documentation"
}
```

The schema requires a title and permits a description. The API currently returns the created document for POST, the updated document for PUT, a list for GET, and an empty 204 response for DELETE.

## Data model

A Mongoose model named `Todo` is defined in `server.js`. Its schema includes:
- `title`: required string.
- `description`: optional string.

MongoDB assigns each document an `_id`. The update and delete routes use this identifier.

## Current limitations

- No user accounts, authentication, or authorization.
- Every caller can list, create, update, and delete records.
- CORS is enabled broadly.
- MongoDB URI and port are hard-coded.
- Input validation and request size limits are minimal.
- Delete returns 204 even if no matching document exists.
- No pagination, sorting, search, or ownership model.
- Error responses are not standardized.
- Root npm scripts and dependencies are incomplete for the tracked React client.
- The current test script is a placeholder that exits with an error.

Do not use this service with private or sensitive tasks until authentication, authorization, validation, and deployment configuration are implemented.

## Documentation

- [Architecture and request lifecycle](docs/ARCHITECTURE.md)
- [API reference](docs/API.md)
- [Data model](docs/DATA-MODEL.md)
- [Development guide](docs/DEVELOPMENT.md)
- [Deployment guide](docs/DEPLOYMENT.md)
- [Testing strategy](docs/TESTING.md)
- [Troubleshooting](docs/TROUBLESHOOTING.md)
- [Security policy](SECURITY.md)

## CI

The current CI workflow checks the syntax of `server.js`. This does not test MongoDB connectivity, API behavior, frontend compilation, or CRUD correctness. A more complete pipeline should be introduced after the client package configuration and integration test setup are made coherent.

Last updated: 2026-11-14 00:11 UTC
