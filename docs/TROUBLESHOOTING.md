# Troubleshooting

## MongoDB connection refused

Confirm MongoDB is running and listening on the host and port in the connection string. The current source expects a local MongoDB service. If using a remote database, change the configuration through a secure environment-variable implementation rather than committing credentials.

## API does not start

Check Node.js version, run `node --check server.js`, install dependencies, and inspect the terminal error. Ensure port 8000 is not already occupied.

## API returns 500

Inspect the sanitized server log. Common causes include database connectivity, invalid MongoDB IDs, malformed request data, or model validation errors. Do not expose raw stack traces to end users.

## Browser request blocked by CORS

The current server uses permissive CORS middleware. If deployment introduces an origin allowlist, ensure the exact frontend origin is configured, including scheme and port. Do not solve CORS problems by allowing every origin in production.

## Frontend command is missing

The tracked source resembles a React application, but the root package manifest does not clearly define a complete React toolchain. Inspect package scripts and dependencies. Establish a coherent client build setup before assuming Create React App commands are available.

## npm ci fails

Check that package.json and package-lock.json are synchronized. The current package manifest contains a local self-reference dependency. Inspect the npm error before deleting or regenerating the lockfile.

## Updated record is not returned

Check the MongoDB ID, request body field names, and Mongoose update behavior. The current update route uses `findByIdAndUpdate`; update validators are not explicitly enabled.

## Delete reports success for a missing ID

The current delete route does not inspect the result of `findByIdAndDelete`. It can return 204 even if no record was deleted. This is a known behavior that requires a source-code fix.

## CI passes but application is not usable

The current CI only checks backend syntax. It does not build the React client or test MongoDB integration. A passing workflow is not a production readiness signal.
