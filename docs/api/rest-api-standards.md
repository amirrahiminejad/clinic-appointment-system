# REST API Standard

## 1. Base URL

```text
https://{domain}/api/{version}
```

Example:

```text
https://api.example.com/api/v1
```

Versioning:

```text
/api/v1
/api/v2
```

---

# 2. Endpoint Naming

### General Rules

* استفاده از **Noun** به جای Verb
* استفاده از **Plural Noun**
* استفاده از `kebab-case`
* عدم استفاده از فعل در Endpoint
* عدم استفاده از `_`
* عدم استفاده از نام متد HTTP در URL

### Standard

```text
GET    /resources
GET    /resources/{id}
POST   /resources
PUT    /resources/{id}
PATCH  /resources/{id}
DELETE /resources/{id}
```

### Example

```text
GET    /users
GET    /users/{userId}
POST   /users
PUT    /users/{userId}
PATCH  /users/{userId}
DELETE /users/{userId}
```

### Nested Resources

```text
GET    /users/{userId}/orders
GET    /users/{userId}/orders/{orderId}
POST   /users/{userId}/orders
```

Maximum nesting:

```text
/users/{userId}/orders/{orderId}
```

از nesting بیشتر از دو سطح استفاده نشود.

---

# 3. HTTP Methods

| Method | Usage                  |
| ------ | ---------------------- |
| GET    | دریافت Resource        |
| POST   | ایجاد Resource         |
| PUT    | جایگزینی کامل Resource |
| PATCH  | تغییر بخشی از Resource |
| DELETE | حذف Resource           |

---

# 4. HTTP Status Codes

## Success

| Status         | Usage                              |
| -------------- | ---------------------------------- |
| 200 OK         | درخواست موفق                       |
| 201 Created    | ایجاد موفق Resource                |
| 202 Accepted   | درخواست پذیرفته شده و پردازش Async |
| 204 No Content | موفق بدون Response Body            |

## Client Errors

| Status                   | Usage             |
| ------------------------ | ----------------- |
| 400 Bad Request          | Request نامعتبر   |
| 401 Unauthorized         | عدم احراز هویت    |
| 403 Forbidden            | عدم دسترسی        |
| 404 Not Found            | Resource یافت نشد |
| 409 Conflict             | Conflict          |
| 422 Unprocessable Entity | Validation Error  |
| 429 Too Many Requests    | Rate Limit        |

## Server Errors

| Status                    | Usage               |
| ------------------------- | ------------------- |
| 500 Internal Server Error | خطای داخلی          |
| 502 Bad Gateway           | خطای Gateway        |
| 503 Service Unavailable   | سرویس در دسترس نیست |
| 504 Gateway Timeout       | Timeout             |

---

# 5. Request Headers

```http
Authorization: Bearer {accessToken}
Content-Type: application/json
Accept: application/json
X-Request-Id: {uuid}
```

Optional:

```http
Accept-Language: fa-IR
X-Client-Version: 1.0.0
X-Platform: web
```

---

# 6. Query Parameters

### Pagination

```text
?page=1&pageSize=20
```

### Sorting

```text
?sortBy=createdAt&sortOrder=desc
```

### Filtering

```text
?status=active
?status=active&role=admin
```

### Search

```text
?search=john
```

### Example

```http
GET /users?page=1&pageSize=20&sortBy=createdAt&sortOrder=desc&status=active
```

---

# 7. Pagination Response

```json
{
  "data": [],
  "pagination": {
    "page": 1,
    "pageSize": 20,
    "totalItems": 100,
    "totalPages": 5,
    "hasNext": true,
    "hasPrevious": false
  }
}
```

---

# 8. Single Resource Response

```json
{
  "data": {
    "id": "01JABC123",
    "name": "John Doe",
    "createdAt": "2026-09-10T12:30:00Z",
    "updatedAt": "2026-09-10T12:30:00Z"
  }
}
```

---

# 9. Collection Response

```json
{
  "data": [
    {
      "id": "01JABC123",
      "name": "John Doe"
    },
    {
      "id": "01JABC124",
      "name": "Jane Doe"
    }
  ],
  "pagination": {
    "page": 1,
    "pageSize": 20,
    "totalItems": 2,
    "totalPages": 1,
    "hasNext": false,
    "hasPrevious": false
  }
}
```

---

# 10. Create Request

```http
POST /users
Content-Type: application/json
```

```json
{
  "name": "John Doe",
  "email": "john@example.com"
}
```

Response:

```http
HTTP/1.1 201 Created
```

```json
{
  "data": {
    "id": "01JABC123",
    "name": "John Doe",
    "email": "john@example.com",
    "createdAt": "2026-09-10T12:30:00Z"
  }
}
```

---

# 11. Update Request

## PUT

```http
PUT /users/{userId}
```

```json
{
  "name": "John Doe",
  "email": "john@example.com"
}
```

## PATCH

```http
PATCH /users/{userId}
```

```json
{
  "email": "new-email@example.com"
}
```

---

# 12. Delete Response

```http
DELETE /users/{userId}
```

Response:

```http
204 No Content
```

Response Body:

```text
Empty
```

---

# 13. Error Response

تمامی Error Responseها باید ساختار یکسان داشته باشند.

```json
{
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User not found.",
    "details": null,
    "traceId": "01JABC123"
  }
}
```

---

# 14. Validation Error

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "One or more validation errors occurred.",
    "details": [
      {
        "field": "email",
        "code": "INVALID_FORMAT",
        "message": "Invalid email format."
      },
      {
        "field": "name",
        "code": "REQUIRED",
        "message": "Name is required."
      }
    ],
    "traceId": "01JABC123"
  }
}
```

---

# 15. Error Code Naming

Format:

```text
{DOMAIN}_{ERROR}
```

Examples:

```text
USER_NOT_FOUND
USER_ALREADY_EXISTS
USER_INVALID_STATUS

ORDER_NOT_FOUND
ORDER_ALREADY_EXISTS
ORDER_INVALID_STATUS

AUTH_INVALID_TOKEN
AUTH_TOKEN_EXPIRED
AUTH_ACCESS_DENIED

VALIDATION_ERROR
RESOURCE_NOT_FOUND
RESOURCE_CONFLICT
INTERNAL_ERROR
```

Rules:

```text
UPPER_SNAKE_CASE
```

---

# 16. Error Object

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message.",
    "details": null,
    "traceId": "request-trace-id"
  }
}
```

Fields:

| Field   | Required | Type                  |
| ------- | -------- | --------------------- |
| code    | Yes      | string                |
| message | Yes      | string                |
| details | No       | object / array / null |
| traceId | Yes      | string                |

---

# 17. Date & Time

تمامی DateTimeها:

```text
ISO 8601
UTC
```

Format:

```text
2026-09-10T12:30:00Z
```

---

# 18. Naming Convention

## JSON Fields

```text
camelCase
```

Correct:

```json
{
  "firstName": "John",
  "lastName": "Doe",
  "createdAt": "2026-09-10T12:30:00Z"
}
```

Incorrect:

```json
{
  "first_name": "John",
  "created_at": "..."
}
```

## URL

```text
kebab-case
```

Correct:

```text
/user-profiles
/order-items
```

Incorrect:

```text
/userProfiles
/user_profiles
```

## IDs

```text
id
```

Resource-specific IDs:

```text
userId
orderId
productId
```

---

# 19. Boolean Fields

Boolean fields باید با یکی از Prefixهای زیر نام‌گذاری شوند:

```text
is
has
can
should
```

Examples:

```json
{
  "isActive": true,
  "isVerified": false,
  "hasPermission": true,
  "canEdit": false
}
```

---

# 20. Enum Values

Enum values باید:

```text
UPPER_SNAKE_CASE
```

Example:

```json
{
  "status": "PENDING"
}
```

Examples:

```text
ACTIVE
INACTIVE
PENDING
APPROVED
REJECTED
CANCELLED
COMPLETED
```

---

# 21. Filtering

Standard:

```text
GET /users?status=ACTIVE
```

Multiple values:

```text
GET /users?status=ACTIVE,PENDING
```

Range:

```text
GET /orders?createdFrom=2026-01-01&createdTo=2026-01-31
```

---

# 22. Sorting

```text
?sortBy=createdAt&sortOrder=desc
```

Allowed values:

```text
asc
desc
```

---

# 23. Field Selection

```text
?fields=id,name,email
```

Example:

```http
GET /users?fields=id,name,email
```

---

# 24. Search

```text
GET /users?search=john
```

Search باید روی فیلدهای تعریف‌شده توسط API اعمال شود.

---

# 25. Idempotency

برای عملیات حساس با `POST`:

```http
Idempotency-Key: {uuid}
```

Example:

```http
POST /payments
Idempotency-Key: 01JABC123
```

---

# 26. Request ID / Trace ID

Client:

```http
X-Request-Id: {uuid}
```

Response:

```http
X-Request-Id: {uuid}
```

Error:

```json
{
  "error": {
    "code": "INTERNAL_ERROR",
    "message": "An unexpected error occurred.",
    "details": null,
    "traceId": "01JABC123"
  }
}
```

---

# 27. Authentication

```http
Authorization: Bearer {accessToken}
```

Unauthorized:

```http
401 Unauthorized
```

```json
{
  "error": {
    "code": "AUTH_UNAUTHORIZED",
    "message": "Authentication is required.",
    "details": null,
    "traceId": "01JABC123"
  }
}
```

Forbidden:

```http
403 Forbidden
```

```json
{
  "error": {
    "code": "AUTH_FORBIDDEN",
    "message": "You do not have permission to perform this operation.",
    "details": null,
    "traceId": "01JABC123"
  }
}
```

---

# 28. Resource Not Found

```http
GET /users/{userId}
```

```http
404 Not Found
```

```json
{
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User not found.",
    "details": null,
    "traceId": "01JABC123"
  }
}
```

---

# 29. Conflict

```http
409 Conflict
```

```json
{
  "error": {
    "code": "USER_ALREADY_EXISTS",
    "message": "A user with this email already exists.",
    "details": null,
    "traceId": "01JABC123"
  }
}
```

---

# 30. API Endpoint Structure

هر Endpoint باید در Documentation با ساختار زیر تعریف شود:

```text
METHOD
PATH

Summary
Description

Authentication

Headers

Path Parameters

Query Parameters

Request Body

Response

Status Codes

Errors
```

Example:

```text
POST /api/v1/users

Summary:
Create a new user.

Authentication:
Bearer Token

Headers:
Authorization: Bearer {accessToken}
Content-Type: application/json
X-Request-Id: {uuid}

Request Body:
{
  "name": "John Doe",
  "email": "john@example.com"
}

Response:
201 Created

{
  "data": {
    "id": "01JABC123",
    "name": "John Doe",
    "email": "john@example.com",
    "createdAt": "2026-09-10T12:30:00Z"
  }
}

Errors:
400 VALIDATION_ERROR
401 AUTH_UNAUTHORIZED
409 USER_ALREADY_EXISTS
```

---

# 31. Standard CRUD Endpoint

```text
GET    /api/v1/{resources}
GET    /api/v1/{resources}/{id}
POST   /api/v1/{resources}
PUT    /api/v1/{resources}/{id}
PATCH  /api/v1/{resources}/{id}
DELETE /api/v1/{resources}/{id}
```

Example:

```text
GET    /api/v1/products
GET    /api/v1/products/{productId}
POST   /api/v1/products
PUT    /api/v1/products/{productId}
PATCH  /api/v1/products/{productId}
DELETE /api/v1/products/{productId}
```

---

# 32. Custom Actions

برای عملیات غیر CRUD از Action Endpoint استفاده شود.

Format:

```text
POST /resources/{id}/{action}
```

Examples:

```text
POST /orders/{orderId}/confirm
POST /orders/{orderId}/cancel
POST /users/{userId}/activate
POST /users/{userId}/deactivate
POST /payments/{paymentId}/refund
```

Action باید:

```text
lowercase
kebab-case
```

باشد.

---

# 33. Bulk Operations

Bulk Create:

```text
POST /users/bulk
```

Bulk Update:

```text
PATCH /users/bulk
```

Bulk Delete:

```text
DELETE /users/bulk
```

Request:

```json
{
  "items": [
    {
      "id": "01JABC123",
      "status": "ACTIVE"
    },
    {
      "id": "01JABC124",
      "status": "ACTIVE"
    }
  ]
}
```

---

# 34. Response Envelope

تمام Responseهای دارای Body باید از Envelope استاندارد استفاده کنند:

Success:

```json
{
  "data": {}
}
```

Collection:

```json
{
  "data": [],
  "pagination": {}
}
```

Error:

```json
{
  "error": {}
}
```

---

# 35. Content Type

Request:

```http
Content-Type: application/json
```

Response:

```http
Content-Type: application/json
```

---

# 36. API Documentation

Documentation باید با:

```text
OpenAPI 3.x
```

تعریف شود.

ساختار:

```text
/api/v1
    /users
    /products
    /orders
    /payments
```

هر Endpoint باید شامل موارد زیر باشد:

```text
Summary
Description
Tags
Authentication
Parameters
RequestBody
Responses
Error Codes
Examples
```

---

# 37. Standard API Structure

```text
/api
 └── v1
      ├── users
      │    ├── GET    /
      │    ├── POST   /
      │    ├── GET    /{userId}
      │    ├── PUT    /{userId}
      │    ├── PATCH  /{userId}
      │    ├── DELETE /{userId}
      │    └── POST   /{userId}/activate
      │
      ├── products
      │    ├── GET    /
      │    ├── POST   /
      │    ├── GET    /{productId}
      │    ├── PUT    /{productId}
      │    ├── PATCH  /{productId}
      │    └── DELETE /{productId}
      │
      └── orders
           ├── GET    /
           ├── POST   /
           ├── GET    /{orderId}
           ├── PATCH  /{orderId}
           ├── POST   /{orderId}/confirm
           └── POST   /{orderId}/cancel
```

# 38. Golden Rules

```text
URL:
    kebab-case
    plural nouns
    no verbs

JSON:
    camelCase

Enum:
    UPPER_SNAKE_CASE

Error Code:
    UPPER_SNAKE_CASE

DateTime:
    ISO 8601 UTC

Success:
    { "data": ... }

Error:
    { "error": ... }

Pagination:
    { "data": [...], "pagination": {...} }

Authentication:
    Authorization: Bearer {token}

Tracing:
    X-Request-Id
    traceId

Version:
    /api/v1

Documentation:
    OpenAPI 3.x
```
