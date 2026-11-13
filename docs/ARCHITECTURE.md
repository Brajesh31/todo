# Architecture

The repository combines a React UI and an Express API.

- `src/`: React entrypoint, app component, todo component, styles, and Create React App test scaffolding.
- `server.js`: Express server, MongoDB connection, Mongoose todo schema, and CRUD routes.
- `public/`: static HTML, icons, and web app manifest.
- `package.json`: root npm manifest. Its scripts and dependency declarations are incomplete relative to the tracked React source.

The API stores documents in a MongoDB collection through a Mongoose model with a required title and optional description.
