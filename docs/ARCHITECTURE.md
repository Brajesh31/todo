# Architecture and Application Flow

## 1. Overview

The repository is split into a browser client and a small Express API. The client collects user input and renders todo records. The API accepts JSON requests, uses Mongoose to read or write MongoDB documents, and returns JSON responses.

```text
Browser
  |
  v
React application (src/)
  |
  | HTTP requests to port 8000
  v
Express server (server.js)
  |
  +--> JSON body parser
  |
  +--> CORS middleware
  |
  +--> Todo route handlers
          |
          v
      Mongoose Todo model
          |
          v
       MongoDB
```

There is no authentication layer between the browser and API. Any client that can reach the API can currently call the CRUD endpoints.

## 2. Frontend structure

The tracked React-style client contains:
- `src/index.js`: browser application entrypoint.
- `src/App.js`: root application component.
- `src/Todo.js`: todo component.
- `src/App.css` and `src/index.css`: styling.
- `src/App.test.js`: test scaffold.
- `public/index.html`: HTML entry document.
- `public/manifest.json`: web application metadata.

A second root-level `Todo.js` exists. Its relationship to `src/Todo.js` should be clarified before cleanup. Avoid maintaining two implementations accidentally.

The repository's package manifest does not currently provide a complete, explicit frontend build toolchain. The file layout alone does not guarantee that the client can be started or built.

## 3. Backend structure

The backend is currently concentrated in `server.js`. It:
1. Imports Express, Mongoose, and CORS.
2. Creates the Express application.
3. Enables JSON request parsing.
4. Enables CORS.
5. Connects to the local MongoDB database.
6. Defines the Mongoose todo schema and model.
7. Registers CRUD routes.
8. Starts the server on port 8000.

This compact structure is easy to follow for a small exercise, but route definitions, data model, configuration, and startup behavior are coupled in one file.

## 4. Request lifecycle

### List

A GET request to `/todos` calls `Todo.find()`, retrieves all documents, and returns the result as JSON. There is no pagination or user filter.

### Create

A POST request reads `title` and `description` from the request body, constructs a Mongoose document, saves it, and returns the saved document with status 201.

### Update

A PUT request reads the MongoDB ID from the URL and title/description from the request body. It calls `findByIdAndUpdate` with `new: true` to return the updated record. If no record is found, it returns 404.

The current update operation does not enable Mongoose update validators explicitly. It also does not validate the ID or normalize the payload before calling the database.

### Delete

A DELETE request calls `findByIdAndDelete` using the URL ID and returns status 204. It does not check whether a record was actually found, so a nonexistent ID can still produce the same success response.

## 5. Data boundary

The Mongoose schema currently requires a title and permits a description. The API accepts arbitrary JSON values without a complete validation layer. A robust boundary should trim strings, reject empty titles, cap field lengths, validate MongoDB IDs, and return consistent client errors.

## 6. Error handling

Database exceptions are caught in route handlers and returned as status 500 with a message. The server logs errors to standard output. There is no centralized error middleware, request correlation ID, structured logger, or production-safe error redaction.

## 7. Configuration and lifecycle

The MongoDB URI and port are hard-coded. The application begins connecting to MongoDB and starts the listener without a documented readiness gate. A production service should validate environment configuration, wait for a successful database connection, and shut down gracefully on termination signals.

## 8. Recommended modular structure

As the project grows, consider separating:
- `config/`: environment parsing and database setup.
- `models/`: Mongoose schema definitions.
- `routes/`: HTTP route declarations.
- `controllers/`: request orchestration.
- `middleware/`: validation, errors, and authentication.
- `services/`: domain-level todo operations.
- `tests/`: unit and integration tests.

This is a future design recommendation, not the current repository structure.
