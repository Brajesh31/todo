# Data Model and MongoDB Behavior

## 1. Current schema

The todo model is declared in `server.js` using Mongoose:

- `title`: String, required.
- `description`: String, optional.

Mongoose creates a model named `Todo` from this schema. The MongoDB collection name is derived from the model name according to Mongoose naming conventions.

## 2. Document identity

MongoDB assigns an `_id` to each document. The update and delete routes accept this identifier as a URL parameter and use Mongoose ID-based helpers.

The current implementation does not explicitly validate the identifier format before querying. Invalid ID formats can raise cast errors and become server errors unless handled separately.

## 3. Validation behavior

The schema marks title as required. This is not the same as a complete input policy. A production validation layer should define:
- Whether leading/trailing whitespace is trimmed.
- Whether an empty or whitespace-only title is rejected.
- Maximum title length.
- Maximum description length.
- Allowed character and encoding behavior.
- Whether unknown request properties are rejected.
- Whether title updates can be partial.

The update route should enable Mongoose update validators or load, validate, and save the document through a deliberate service flow.

## 4. Current persistence operations

| Operation | Mongoose operation | Behavior |
|---|---|---|
| List | `find()` | Retrieves all documents |
| Create | `new Model()` + `save()` | Inserts a document |
| Update | `findByIdAndUpdate()` | Updates by ID |
| Delete | `findByIdAndDelete()` | Deletes by ID |

No database index strategy is documented. At the current small scale, this may be acceptable for a demonstration, but larger collections need measured query plans and pagination.

## 5. Multi-user design is absent

There is no user reference in the current todo schema. Every API caller can read and modify every record. If the application becomes multi-user, the schema must include an owner reference and every query must be scoped by the authenticated user. Merely hiding another user's records in React is not security.

## 6. Future schema evolution

Potential future fields could include status, priority, due date, completion timestamp, labels, and owner. Add fields only with clear semantics and validation. For changes to existing records, define migration behavior and default values.

## 7. Backup and privacy

Do not store sensitive personal information in this prototype. Before real use, define backup frequency, retention, encryption, restore testing, and data deletion policies.
