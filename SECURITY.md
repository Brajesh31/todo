# Security Policy

Report suspected vulnerabilities privately through GitHub's security reporting feature where available. Do not include private task records or credentials in public issues.

## Known security limitations

- API routes have no authentication or authorization.
- CORS currently allows requests from any origin.
- MongoDB connection details are hard-coded.
- Request bodies are not comprehensively validated.
- The service has no documented rate limiting or audit logging.

Do not expose this prototype publicly with sensitive data until these issues are addressed.
