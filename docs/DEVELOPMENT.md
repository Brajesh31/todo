# Development

1. Install Node.js and npm.
2. Ensure MongoDB is available.
3. Run `npm ci` from the repository root and resolve any lockfile or local-file dependency issue.
4. Start the API with `node server.js`.
5. Run the React application using the client toolchain configured for this repository.

The root manifest currently declares a placeholder test command that exits with an error. It also does not clearly declare the React build/test dependencies required by the tracked source. Do not treat a successful dependency install as proof that the frontend is buildable. Add a coherent scripts/dependencies setup and tests as a separate implementation change.
