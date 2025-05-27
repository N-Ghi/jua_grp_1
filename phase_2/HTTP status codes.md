# HTTP Status Codes & Error Handling
To ensure consistency, transparency, and developer-friendly usage, the JuaJobs API adheres to industry best practices for HTTP status codes and structured error messaging. This supports better debugging, smoother integrations, and clear communication between API consumers and the backend system.

## HTTP Status Codes Definition
| Code |	Meaning	| Usage in JuaJobs API |
|--------|---------|---------------------|
|200 OK |	Successful request |	Resource retrieval (GET), job listing, profile fetch |
|201 Created |	Resource successfully created	| User registration, job posting, application submission |
|204 No Content |	Successful with no body |	DELETE operations (e.g., delete review or job post) |
|400 Bad Request |	Invalid input from client	| Malformed JSON, missing fields, invalid parameter values |
|401 Unauthorized	| No valid authentication	| Missing or invalid access token |
|403 Forbidden |	Authenticated but no permission |	Worker trying to delete a client’s job post |
|404 Not Found |	Resource doesn’t exist |	Requesting a non-existent job, profile, or endpoint |
|409 Conflict |	Resource state conflict	| Duplicate application to the same job by the same worker |
|422 Unprocessable Entity	| Validation failure | Failing to meet business rules like unsupported job categories or invalid mobile numbers |
|500 Internal Server Error |	Unhandled exceptions	| Unexpected server-side issues |
|503 Service Unavailable |	Temporary outage |	Downtime for database or external services |

## Standardized Error Response Format
All API error responses will follow a consistent structure for easy parsing and user feedback:

'''{
  "status": 400,
  "error": "Bad Request",
  "code": "VALIDATION_FAILED",
  "message": "The 'email' field is required and must be a valid email address.",
  "timestamp": "2025-05-26T15:45:00Z",
  "path": "/api/v1/users/register"
}'''

## Error Categorization
| Category |	Prefix Code |	Example Codes |	Description |
|-----------|--------------|--------------|-------------|
| Validation Errors	| VALIDATION_	| VALIDATION_FAILED, VALIDATION_MISSING_FIELD |	Missing or improperly formatted input. |
| Business Logic Errors |	BUSINESS_ |	BUSINESS_DUPLICATE_APPLICATION, BUSINESS_UNAVAILABLE_WORKER |	Domain-specific constraints. |
| Authentication/Authorization |	AUTH_	| AUTH_INVALID_TOKEN, AUTH_FORBIDDEN_ROLE |	Session and permission-related failures. |
| System Errors |	SYSTEM_ |	SYSTEM_DATABASE_ERROR, SYSTEM_TIMEOUT |	Infrastructure or unexpected runtime issues. |

## Design of Informative Error Messages
Error messages are structured to be human-readable while still machine-parsable. Guidelines include:

Clear explanation of what went wrong.

Where applicable, which field or parameter caused the issue.

Suggestions for corrective action.

Example: "The 'phone_number' must be a valid E.164 format starting with +."

