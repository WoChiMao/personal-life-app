# Personal Life App — API Specification

## 1. Purpose

This document defines the HTTP API contract for the Personal Life App.

The API acts as the communication layer between the mobile application and the backend services.

The primary responsibilities of the API are:

- Authenticate users
- Authorize access to user-owned resources
- Create and manage personal data
- Validate requests
- Retrieve data
- Manage reminders
- Manage notifications
- Provide consistent errors
- Support pagination, filtering, and sorting
- Protect user data

The API must never expose direct database access to the mobile application.

Architecture:

```text
React Native App
       │
       │ HTTPS / JSON
       ▼
Backend REST API
       │
       ▼
Application / Service Layer
       │
       ▼
PostgreSQL

---

# 2. API Style

The MVP will use:

```text
REST API
```

### Communication Format

```text
JSON
```

### Transport

```text
HTTPS
```

### HTTP Methods

- GET
- POST
- PATCH
- DELETE

The API will use resource-oriented endpoints.

---

# 3. API Base URL

### Development

```text
http://localhost:<PORT>/api/v1
```

### Production

```text
https://api.<production-domain>/api/v1
```

The actual production domain will be defined during deployment.

---

# 4. API Versioning

The API will use URL-based versioning.

### Example

```text
/api/v1/tasks
```

Future versions may use:

```text
/api/v2/tasks
```

The mobile application should not depend on unversioned endpoints.

---

# 5. Content Type

Requests containing JSON must use:

```http
Content-Type: application/json
```

### Example

```http
Content-Type: application/json
```

Responses will normally use:

```http
Content-Type: application/json
```

---

# 6. Authentication

The API will use token-based authentication.

The MVP architecture will support:

```text
Access Token
+
Refresh Token
```

The exact authentication library/provider will be finalized during implementation.

### Conceptually

```text
User
 │
 ▼
Login
 │
 ▼
Backend
 │
 ├── Access Token
 └── Refresh Token
```

---

# 7. Authorization Header

Authenticated requests will use:

```http
Authorization: Bearer <access_token>
```

### Example

```http
Authorization: Bearer eyJ...
```

The mobile application must attach the access token to protected requests.

---

# 8. Public Endpoints

The following endpoints do not require authentication:

```http
POST /auth/register
POST /auth/login
POST /auth/refresh
```

Other endpoints are protected unless explicitly stated otherwise.

---

# 9. Protected Endpoints

Protected endpoints require:

```http
Authorization: Bearer <access_token>
```

### Examples

```http
GET /users/me
GET /tasks
POST /tasks
GET /bills
POST /documents
```

---

# 10. Authentication Endpoints

## 10.1 Register

```http
POST /api/v1/auth/register
```

### Purpose

Creates a new user account.

### Request

```json
{
  "email": "user@example.com",
  "password": "SecurePassword123",
  "firstName": "John",
  "lastName": "Doe",
  "timezone": "Asia/Manila"
}
```

### Response

```json
{
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "firstName": "John",
    "lastName": "Doe",
    "timezone": "Asia/Manila"
  },
  "accessToken": "access-token",
  "refreshToken": "refresh-token"
}
```

### Status

```http
201 Created
```

---

# 11. Login

```http
POST /api/v1/auth/login
```

### Request

```json
{
  "email": "user@example.com",
  "password": "SecurePassword123"
}
```

### Response

```json
{
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "firstName": "John",
    "lastName": "Doe",
    "timezone": "Asia/Manila"
  },
  "accessToken": "access-token",
  "refreshToken": "refresh-token"
}
```

### Status

```http
200 OK
```

---

# 12. Refresh Token

```http
POST /api/v1/auth/refresh
```

### Request

```json
{
  "refreshToken": "refresh-token"
}
```

### Response

```json
{
  "accessToken": "new-access-token"
}
```

### Status

```http
200 OK
```

---

# 13. Logout

```http
POST /api/v1/auth/logout
```

### Authentication

Required

### Request

```json
{
  "refreshToken": "refresh-token"
}
```

### Response

```json
{
  "message": "Logged out successfully"
}
```

### Status

```http
200 OK
```

The backend should invalidate the refresh token/session where applicable.

---

# 14. Get Current User

```http
GET /api/v1/users/me
```

### Authentication

Required

### Response

```json
{
  "id": "uuid",
  "email": "user@example.com",
  "firstName": "John",
  "lastName": "Doe",
  "timezone": "Asia/Manila",
  "createdAt": "2026-08-24T10:00:00Z"
}
```

### Status

```http
200 OK
```

---

# 15. Update Current User

```http
PATCH /api/v1/users/me
```

### Authentication

Required

### Request

```json
{
  "firstName": "John",
  "lastName": "Smith",
  "timezone": "America/New_York"
}
```

### Response

```json
{
  "id": "uuid",
  "email": "user@example.com",
  "firstName": "John",
  "lastName": "Smith",
  "timezone": "America/New_York"
}
```

### Status

```http
200 OK
```

Email changes may require a separate verification workflow in the future.

---

# 16. Categories API

Categories are user-owned resources.

### Base Endpoint

```http
/api/v1/categories
```

---

# 17. Get Categories

```http
GET /api/v1/categories
```

### Authentication

Required

### Response

```json
{
  "data": [
    {
      "id": "uuid",
      "name": "Finance",
      "color": "#000000",
      "icon": "wallet",
      "createdAt": "2026-08-24T10:00:00Z",
      "updatedAt": "2026-08-24T10:00:00Z"
    }
  ]
}
```

### Status

```http
200 OK
```

---

# 18. Create Category

```http
POST /api/v1/categories
```

### Request

```json
{
  "name": "Finance",
  "color": "#000000",
  "icon": "wallet"
}
```

### Response

```json
{
  "id": "uuid",
  "name": "Finance",
  "color": "#000000",
  "icon": "wallet",
  "createdAt": "2026-08-24T10:00:00Z",
  "updatedAt": "2026-08-24T10:00:00Z"
}
```

### Status

```http
201 Created
```

---

# 19. Update Category

```http
PATCH /api/v1/categories/:id
```

### Request

```json
{
  "name": "Personal Finance",
  "color": "#000000",
  "icon": "wallet"
}
```

### Response

```json
{
  "id": "uuid",
  "name": "Personal Finance",
  "color": "#000000",
  "icon": "wallet"
}
```

### Status

```http
200 OK
```

---

# 20. Delete Category

```http
DELETE /api/v1/categories/:id
```

### Response

```json
{
  "message": "Category deleted successfully"
}
```

### Status

```http
200 OK
```

If tasks, bills, or documents reference the category, the backend must handle the relationship safely.

The MVP should not blindly delete dependent records.

---

# 21. Tasks API

### Base Endpoint

```http
/api/v1/tasks
```

Tasks are user-owned resources.

---

# 22. Get Tasks

```http
GET /api/v1/tasks
```

### Authentication

Required

### Example

```http
GET /api/v1/tasks?status=ACTIVE&priority=HIGH
```

### Response

```json
{
  "data": [
    {
      "id": "uuid",
      "title": "Renew driver's license",
      "description": "Prepare required documents",
      "status": "ACTIVE",
      "priority": "HIGH",
      "dueDate": "2027-01-15",
      "category": {
        "id": "uuid",
        "name": "Documents"
      },
      "isCompleted": false,
      "createdAt": "2026-08-24T10:00:00Z",
      "updatedAt": "2026-08-24T10:00:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "pageSize": 20,
    "totalItems": 1,
    "totalPages": 1
  }
}
```

---

# 23. Task Query Parameters

Supported MVP parameters:

- page
- pageSize
- status
- priority
- categoryId
- dueDate
- fromDate
- toDate
- sortBy
- sortOrder

### Example

```http
GET /api/v1/tasks?page=1&pageSize=20&status=ACTIVE
```

---

# 24. Create Task

```http
POST /api/v1/tasks
```

### Request

```json
{
  "title": "Renew driver's license",
  "description": "Prepare required documents",
  "categoryId": "uuid",
  "priority": "HIGH",
  "dueDate": "2027-01-15"
}
```

### Response

```json
{
  "id": "uuid",
  "title": "Renew driver's license",
  "description": "Prepare required documents",
  "categoryId": "uuid",
  "status": "ACTIVE",
  "priority": "HIGH",
  "dueDate": "2027-01-15",
  "isCompleted": false,
  "createdAt": "2026-08-24T10:00:00Z",
  "updatedAt": "2026-08-24T10:00:00Z"
}
```

### Status

```http
201 Created
```

The backend determines:

- userId
- status
- isCompleted
- createdAt
- updatedAt

The client should not be trusted to provide ownership/system fields.

---

# 25. Get Task

```http
GET /api/v1/tasks/:id
```

### Response

```json
{
  "id": "uuid",
  "title": "Renew driver's license",
  "description": "Prepare required documents",
  "status": "ACTIVE",
  "priority": "HIGH",
  "dueDate": "2027-01-15",
  "isCompleted": false
}
```

### Status

```http
200 OK
```

If the task does not belong to the authenticated user:

```http
404 Not Found
```

The API should avoid revealing that another user's record exists.

---

# 26. Update Task

```http
PATCH /api/v1/tasks/:id
```

### Request

```json
{
  "title": "Renew driver's license",
  "priority": "MEDIUM",
  "dueDate": "2027-01-20"
}
```

Only provided fields are updated.

### Response

```json
{
  "id": "uuid",
  "title": "Renew driver's license",
  "priority": "MEDIUM",
  "dueDate": "2027-01-20"
}
```

### Status

```http
200 OK
```

---

# 27. Complete Task

The MVP may support completion through:

```http
PATCH /api/v1/tasks/:id
```

### Request

```json
{
  "status": "COMPLETED"
}
```

The backend should automatically set:

```text
is_completed = true
completed_at = current timestamp
```

---

# 28. Delete Task

```http
DELETE /api/v1/tasks/:id
```

### Response

```json
{
  "message": "Task deleted successfully"
}
```

### Status

```http
200 OK
```

---

# 29. Bills API

### Base Endpoint

```http
/api/v1/bills
```

---

# 30. Get Bills

```http
GET /api/v1/bills
```

### Example

```http
GET /api/v1/bills?status=UPCOMING
```

### Response

```json
{
  "data": [
    {
      "id": "uuid",
      "name": "Internet Bill",
      "amount": 2500.0,
      "currency": "PHP",
      "dueDate": "2026-09-05",
      "status": "UPCOMING",
      "isAutopay": false
    }
  ],
  "pagination": {
    "page": 1,
    "pageSize": 20,
    "totalItems": 1,
    "totalPages": 1
  }
}
```

---

# 31. Create Bill

```http
POST /api/v1/bills
```

### Request

```json
{
  "name": "Internet Bill",
  "description": "Monthly internet payment",
  "categoryId": "uuid",
  "amount": 2500.0,
  "currency": "PHP",
  "dueDate": "2026-09-05",
  "isAutopay": false
}
```

### Response

```json
{
  "id": "uuid",
  "name": "Internet Bill",
  "amount": 2500.0,
  "currency": "PHP",
  "dueDate": "2026-09-05",
  "status": "UPCOMING",
  "isAutopay": false
}
```

### Status

```http
201 Created
```

---

# 32. Get Bill

```http
GET /api/v1/bills/:id
```

### Status

```http
200 OK
```

---

# 33. Update Bill

```http
PATCH /api/v1/bills/:id
```

### Request

```json
{
  "amount": 2700.00,
  "dueDate": "2026-09-05"
}
```

### Status

```http
200 OK
```

---

# 34. Mark Bill as Paid

```http
PATCH /api/v1/bills/:id
```

### Request

```json
{
  "status": "PAID"
}
```

The backend should automatically set:

```text
paid_at
```

---

# 35. Delete Bill

```http
DELETE /api/v1/bills/:id
```

### Response

```json
{
  "message": "Bill deleted successfully"
}
```

### Status

```http
200 OK
```

---

# 36. Documents API

### Base Endpoint

```http
/api/v1/documents
```

---

# 37. Get Documents

```http
GET /api/v1/documents
```

### Example

```http
GET /api/v1/documents?status=ACTIVE
```

### Response

```json
{
  "data": [
    {
      "id": "uuid",
      "name": "Driver's License",
      "issueDate": "2022-01-15",
      "expirationDate": "2027-01-15",
      "status": "ACTIVE"
    }
  ],
  "pagination": {
    "page": 1,
    "pageSize": 20,
    "totalItems": 1,
    "totalPages": 1
  }
}
```

---

# 38. Create Document

```http
POST /api/v1/documents
```

### Request

```json
{
  "name": "Driver's License",
  "description": "Personal driver's license",
  "categoryId": "uuid",
  "documentNumber": "REDACTED",
  "issueDate": "2022-01-15",
  "expirationDate": "2027-01-15"
}
```

### Response

```json
{
  "id": "uuid",
  "name": "Driver's License",
  "issueDate": "2022-01-15",
  "expirationDate": "2027-01-15",
  "status": "ACTIVE"
}
```

### Status

```http
201 Created
```

---

# 39. Get Document

```http
GET /api/v1/documents/:id
```

### Status

```http
200 OK
```

Sensitive fields should only be returned when necessary.

---

# 40. Update Document

```http
PATCH /api/v1/documents/:id
```

### Request

```json
{
  "expirationDate": "2028-01-15"
}
```

### Status

```http
200 OK
```

---

# 41. Delete Document

```http
DELETE /api/v1/documents/:id
```

### Response

```json
{
  "message": "Document deleted successfully"
}
```

### Status

```http
200 OK
```

---

# 42. Reminders API

### Base Endpoint

```http
/api/v1/reminders
```

---

# 43. Get Reminders

```http
GET /api/v1/reminders
```

### Example

```http
GET /api/v1/reminders?status=PENDING
```

### Response

```json
{
  "data": [
    {
      "id": "uuid",
      "title": "Driver's license expires soon",
      "reminderAt": "2026-12-16T09:00:00Z",
      "status": "PENDING",
      "documentId": "uuid"
    }
  ],
  "pagination": {
    "page": 1,
    "pageSize": 20,
    "totalItems": 1,
    "totalPages": 1
  }
}
```

---

# 44. Create Reminder

```http
POST /api/v1/reminders
```

### Request (Document)

```json
{
  "documentId": "uuid",
  "title": "Driver's license expires soon",
  "reminderAt": "2026-12-16T09:00:00Z"
}
```

### Alternative (Task)

```json
{
  "taskId": "uuid",
  "title": "Task reminder",
  "reminderAt": "2026-08-30T09:00:00Z"
}
```

### Alternative (Bill)

```json
{
  "billId": "uuid",
  "title": "Internet bill due soon",
  "reminderAt": "2026-09-03T09:00:00Z"
}
```

Exactly one target must be provided.

### Status

```http
201 Created
```

---

# 45. Get Reminder

```http
GET /api/v1/reminders/:id
```

### Status

```http
200 OK
```

---

# 46. Update Reminder

```http
PATCH /api/v1/reminders/:id
```

### Request

```json
{
  "reminderAt": "2026-12-15T09:00:00Z"
}
```

### Status

```http
200 OK
```

---

# 47. Cancel Reminder

```http
PATCH /api/v1/reminders/:id
```

### Request

```json
{
  "status": "CANCELLED"
}
```

---

# 48. Delete Reminder

```http
DELETE /api/v1/reminders/:id
```

### Response

```json
{
  "message": "Reminder deleted successfully"
}
```

---

# 49. Notifications API

### Base Endpoint

```http
/api/v1/notifications
```

---

# 50. Get Notifications

```http
GET /api/v1/notifications
```

### Example

```http
GET /api/v1/notifications?isRead=false
```

### Response

```json
{
  "data": [
    {
      "id": "uuid",
      "type": "DOCUMENT_EXPIRATION",
      "title": "Document Expiring Soon",
      "message": "Your driver's license expires in 30 days.",
      "isRead": false,
      "createdAt": "2026-08-24T10:00:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "pageSize": 20,
    "totalItems": 1,
    "totalPages": 1
  }
}
```

---

# 51. Mark Notification as Read

```http
PATCH /api/v1/notifications/:id/read
```

### Response

```json
{
  "id": "uuid",
  "isRead": true,
  "readAt": "2026-08-24T12:00:00Z"
}
```

### Status

```http
200 OK
```

---

# 52. Mark All Notifications as Read

```http
PATCH /api/v1/notifications/read-all
```

### Response

```json
{
  "message": "Notifications marked as read"
}
```

### Status

```http
200 OK
```

---

# 53. Pagination

List endpoints must support pagination.

### Parameters

- page
- pageSize

### Example

```http
?page=1&pageSize=20
```

### Default Values

```text
page = 1
pageSize = 20
```

The backend should enforce a maximum page size.

### Example

```text
maximum pageSize = 100
```

The exact limit can be adjusted during implementation.

---

# 54. Pagination Response

### Standard Structure

```json
{
  "data": [],
  "pagination": {
    "page": 1,
    "pageSize": 20,
    "totalItems": 100,
    "totalPages": 5
  }
}
```

---

# 55. Sorting

List endpoints should support:

- sortBy
- sortOrder

### Example

```http
GET /api/v1/tasks?sortBy=dueDate&sortOrder=asc
```

Allowed values must be explicitly validated by the backend.

The backend must never directly interpolate arbitrary client input into SQL.

---

# 56. Filtering

Filtering should use query parameters.

### Example

```http
GET /api/v1/tasks?status=ACTIVE
```

### Multiple Filters

```http
GET /api/v1/tasks?status=ACTIVE&priority=HIGH
```

The backend must validate filter values.

---

# 57. Date Filtering

### Example

```http
GET /api/v1/tasks?fromDate=2026-08-24&toDate=2026-08-31
```

This can be used to retrieve tasks within a date range.

---

# 58. Search

The MVP may support simple search for tasks.

### Example

```http
GET /api/v1/tasks?search=license
```

The backend may initially use PostgreSQL text matching.

A dedicated search engine is not required for the MVP.

---

# 59. Error Response Format

All API errors should use a consistent structure.

### Example

```json
{
  "error": {
    "code": "TASK_NOT_FOUND",
    "message": "Task not found",
    "details": null
  }
}
```

---

# 60. Validation Error

### Example

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Request validation failed",
    "details": {
      "title": "Title is required",
      "priority": "Invalid priority"
    }
  }
}
```

### Status

```http
400 Bad Request
```

---

# 61. Common HTTP Status Codes

| Status | Meaning |
|----------|---------|
| 200 | Successful Request |
| 201 | Resource Created |
| 204 | Success With No Response Body |
| 400 | Invalid Request |
| 401 | Authentication Required/Invalid |
| 403 | Access Denied |
| 404 | Resource Not Found |
| 409 | Resource Conflict |
| 422 | Validation Failure |
| 429 | Rate Limit Exceeded |
| 500 | Internal Server Error |

---

# 62. Authentication Errors

### Example

```json
{
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Authentication required",
    "details": null
  }
}
```

### Status

```http
401 Unauthorized
```

---

# 63. Authorization Errors

When the authenticated user does not have permission:

```json
{
  "error": {
    "code": "FORBIDDEN",
    "message": "You do not have permission to access this resource",
    "details": null
  }
}
```

### Status

```http
403 Forbidden
```

However, for user-owned resources, the API may intentionally return:

```http
404 Not Found
```

rather than revealing another user's resource exists.

---

# 64. Not Found Errors

### Example

```json
{
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "Resource not found",
    "details": null
  }
}
```

### Status

```http
404 Not Found
```

---

# 65. Conflict Errors

### Example

```json
{
  "error": {
    "code": "EMAIL_ALREADY_EXISTS",
    "message": "An account with this email already exists",
    "details": null
  }
}
```

### Status

```http
409 Conflict
```

---

# 66. Internal Server Errors

Production responses should not expose:

- Stack traces
- SQL statements
- Database credentials
- Internal implementation details
- File paths

### Example

```json
{
  "error": {
    "code": "INTERNAL_SERVER_ERROR",
    "message": "Something went wrong",
    "details": null
  }
}
```

### Status

```http
500 Internal Server Error
```

Detailed errors should be logged securely on the server.

---

# 67. Request Validation

All incoming requests must be validated by the backend.

### Example Task Validation

```text
title
    required
    max length

priority
    LOW | MEDIUM | HIGH

dueDate
    valid date
```

Never rely exclusively on React Native validation.

---

# 68. Authentication vs Authorization

Authentication answers:

> Who is the user?

Authorization answers:

> What is the user allowed to access?

The backend must perform both.

### Example

```text
Token
  ↓
Identify user
  ↓
Find task
  ↓
Verify task.user_id
  ↓
Allow access
```

---

# 69. User Ownership

The backend must derive the authenticated user from the access token.

The client must not be allowed to create:

```json
{
  "userId": "another-user-id"
}
```

and assign ownership.

The backend determines:

```text
user_id = authenticatedUser.id
```

---

# 70. Resource Ownership Rules

The following resources are user-owned:

- categories
- tasks
- bills
- documents
- reminders
- notifications

Users may only access their own records.

---

# 71. API Response Naming

### Database

Uses:

```text
snake_case
```

Example:

```text
created_at
due_date
is_completed
```

### API

Uses:

```text
camelCase
```

Example:

```json
{
  "createdAt": "...",
  "dueDate": "...",
  "isCompleted": false
}
```

The backend is responsible for translating between the two representations.

---

# 72. Date Format

### Date-Only Values

Format:

```text
YYYY-MM-DD
```

### Example

```text
2026-08-24
```

### Timestamps

Format:

```text
ISO 8601
```

### Example

```text
2026-08-24T10:30:00Z
```

---

# 73. Timezone Handling

The user's timezone will be stored in the user profile.

### Example

```text
Asia/Manila
```

The backend should store timestamps consistently.

Reminder processing must consider the user's timezone.

### Example

```text
User Timezone:
Asia/Manila

Reminder:
9:00 AM Local Time
```

The system must not assume every user is in UTC.

---

# 74. Idempotency

Some operations may eventually require idempotency.

### Examples

- Payment Processing
- External Integrations
- Notification Processing

The MVP does not require idempotency keys for every endpoint.

They should be introduced when a specific operation requires them.

---

# 75. Rate Limiting

Production APIs should implement rate limiting.

### Potential Limits

#### Authentication

```text
Strict
```

#### Normal API

```text
Moderate
```

#### Read Endpoints

```text
Higher
```

#### Write Endpoints

```text
Moderate
```

Exact limits will be determined during backend implementation and deployment.

---

# 76. API Security

The API must:

- Require HTTPS in production.
- Validate authentication tokens.
- Validate request bodies.
- Validate query parameters.
- Prevent SQL injection.
- Prevent unauthorized resource access.
- Avoid leaking sensitive information.
- Rate-limit sensitive endpoints.
- Secure refresh tokens.
- Never expose database credentials.

---

# 77. SQL Injection Protection

The backend must use parameterized queries or a trusted ORM/query builder.

### Never

```sql
SELECT * FROM tasks
WHERE title = '${userInput}'
```

Use parameterized queries instead.

---

# 78. CORS

The backend must configure CORS appropriately.

Development may allow local development origins.

Production should allow only trusted origins where applicable.

The mobile application itself is not a browser origin in the same way as a web application, but the API may later support a web client.

---

# 79. API Logging

The backend should log important API events without exposing sensitive information.

### Useful Information

- Request ID
- Endpoint
- HTTP Method
- Status Code
- Response Time
- Authenticated User ID
- Timestamp

### Do NOT Log

- Passwords
- Access Tokens
- Refresh Tokens
- Full Document Numbers
- Sensitive Financial Information

---

# 80. Request ID

The API should eventually support a request identifier.

### Example

```http
X-Request-ID: 4d2a9c...
```

This helps trace problems across:

```text
Mobile
 ↓
API
 ↓
Service
 ↓
Database
```

---

# 81. Health Check

The backend should provide a health endpoint.

```http
GET /api/v1/health
```

### Response

```json
{
  "status": "ok"
}
```

### Status

```http
200 OK
```

The endpoint should not expose sensitive infrastructure information.

---

# 82. API Endpoint Summary

## Authentication

```http
POST   /auth/register
POST   /auth/login
POST   /auth/refresh
POST   /auth/logout
```

## User

```http
GET    /users/me
PATCH  /users/me
```

## Categories

```http
GET    /categories
POST   /categories
PATCH  /categories/:id
DELETE /categories/:id
```

## Tasks

```http
GET    /tasks
GET    /tasks/:id
POST   /tasks
PATCH  /tasks/:id
DELETE /tasks/:id
```

## Bills

```http
GET    /bills
GET    /bills/:id
POST   /bills
PATCH  /bills/:id
DELETE /bills/:id
```

## Documents

```http
GET    /documents
GET    /documents/:id
POST   /documents
PATCH  /documents/:id
DELETE /documents/:id
```

## Reminders

```http
GET    /reminders
GET    /reminders/:id
POST   /reminders
PATCH  /reminders/:id
DELETE /reminders/:id
```

## Notifications

```http
GET    /notifications
PATCH  /notifications/:id/read
PATCH  /notifications/read-all
```

## Health

```http
GET    /health
```

---

# 83. API Design Philosophy

The API should remain:

- Simple
- Predictable
- Secure
- Versioned
- Consistent
- Mobile-Friendly

Avoid unnecessary endpoints.

### Example

We do not need:

```http
POST /complete-task
POST /mark-bill-paid
```

when these operations can safely be represented by:

```http
PATCH /tasks/:id
PATCH /bills/:id
```

unless a future business requirement justifies dedicated actions.

---

# 84. MVP API Scope

The MVP API will support:

- Authentication
- User Profile
- Categories
- Tasks
- Bills
- Documents
- Reminders
- Notifications

### Out of Scope

- Bank Integrations
- Payment Processing
- Family Sharing
- Vehicle APIs
- AI APIs
- External Calendar Integrations
- Cloud File Uploads
- Subscription Integrations
- Advanced Analytics

---

# 85. Future API Expansion

Future endpoints may include:

```text
/households
/vehicles
/subscriptions
/goals
/calendar
/attachments
/integrations
/ai
```

These should be introduced only when their corresponding product features are implemented.

---

# 86. API Testing Strategy

The backend must eventually include tests for:

## Authentication

- Register
- Login
- Refresh
- Logout
- Invalid Credentials
- Expired Tokens

## Tasks

- Create
- Read
- Update
- Delete
- Complete
- Authorization
- Validation
- Pagination
- Filtering

## Bills

- Create
- Read
- Update
- Delete
- Mark Paid
- Authorization
- Validation

## Documents

- Create
- Read
- Update
- Delete
- Expiration Handling
- Authorization

## Reminders

- Create
- Read
- Update
- Delete
- Target Validation

## Notifications

- Read
- Mark Read
- Mark All Read
- Authorization

---

# 87. API Contract Principle

The API specification is a contract.

The React Native application should be able to consume the API without needing to know:

- PostgreSQL Structure
- Database Table Names
- ORM Implementation
- Backend Framework
- Internal Business Logic

### Example

```text
React Native
     │
     ▼
POST /api/v1/tasks
     │
     ▼
Backend
     │
     ▼
PostgreSQL
```

The mobile application only needs to understand the API contract.

---

# 88. Backend Responsibilities

The backend is responsible for:

- Authentication
- Authorization
- Validation
- Business Logic
- Database Access
- Data Transformation
- Error Handling
- Security
- Notifications

---

# 89. Mobile Responsibilities

The React Native application is responsible for:

- User Interface
- Navigation
- Form Handling
- Local UI State
- API Requests
- Loading States
- Error Presentation
- Authentication State
- Local Caching

The mobile application should not contain authoritative business rules that must also be enforced by the backend.

---

# 90. API Layer Architecture

The backend should conceptually follow:

```text
Controller
    ↓
Service
    ↓
Repository / Data Access
    ↓
PostgreSQL
```

### Example

```text
POST /tasks
      ↓
TaskController
      ↓
TaskService
      ↓
TaskRepository
      ↓
PostgreSQL
```

This keeps the code maintainable as the application grows.

---

# 91. Error Handling Principle

Every error should be:

- Predictable
- Machine-Readable
- Human-Readable
- Safe

### Example

```json
{
  "error": {
    "code": "TASK_NOT_FOUND",
    "message": "Task not found",
    "details": null
  }
}
```

The mobile app can use:

```text
error.code
```

for programmatic handling.

---

# 92. API Documentation

The API should eventually be documented using:

```text
OpenAPI / Swagger
```

The OpenAPI specification should become the machine-readable version of this document.

### Example

```text
docs/openapi.yaml
```

This will be introduced during backend implementation.

---

# 93. API Specification Status

The API specification currently represents the intended MVP contract.

Exact implementation details may be refined when:

- Backend Framework is selected
- ORM is selected
- Authentication Provider is selected
- Database Migrations are created
- Automated Tests are implemented

Changes must be reflected in this document.

---

# 94. Phase 7 Completion Criteria

Phase 7 is complete when:

- ✅ API Architecture Defined
- ✅ API Versioning Defined
- ✅ Authentication Endpoints Defined
- ✅ User Endpoints Defined
- ✅ Category Endpoints Defined
- ✅ Task Endpoints Defined
- ✅ Bill Endpoints Defined
- ✅ Document Endpoints Defined
- ✅ Reminder Endpoints Defined
- ✅ Notification Endpoints Defined
- ✅ Pagination Defined
- ✅ Filtering Defined
- ✅ Sorting Defined
- ✅ Validation Strategy Defined
- ✅ Error Format Defined
- ✅ HTTP Status Codes Defined
- ✅ Authorization Strategy Defined
- ✅ User Ownership Rules Defined
- ✅ Security Principles Defined
- ✅ Testing Strategy Defined
- ✅ Future API Expansion Documented

---

# 95. Next Phase

## Phase 8 — Project Initialization

After the API contract is approved, we will finally start setting up the actual development environment.

### Phase 8 Will Cover

- React Native Project
- Backend Project
- PostgreSQL
- Environment Variables
- Git Structure
- ESLint
- Prettier
- TypeScript
- Database Connection
- ORM
- Migrations
- Development Scripts

We will also establish the actual repository structure.

### Example

```text
personal-life-app/
│
├── apps/
│   ├── mobile/
│   └── api/
│
├── docs/
│
├── packages/
│
├── .gitignore
├── README.md
└── package.json
```
