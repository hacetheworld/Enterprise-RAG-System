# API Design

## Overview

This document defines the initial API contract between the frontend and backend applications.

The goal is to provide a clear implementation roadmap while keeping the API surface simple and maintainable.

---

# Authentication Module

## Login

Authenticate a user and return an access token.

### Endpoint

```http
POST /api/auth/login
```

### Request

```json
{
  "email": "ajay@company.com",
  "password": "password123"
}
```

### Response

```json
{
  "token": "jwt-token",
  "user": {
    "id": "user-id",
    "name": "Ajay",
    "role": "EMPLOYEE"
  }
}
```

---

## Current User

Returns information about the authenticated user.

### Endpoint

```http
GET /api/auth/me
```

### Response

```json
{
  "id": "user-id",
  "name": "Ajay",
  "email": "ajay@company.com",
  "role": "EMPLOYEE",
  "departments": [
    {
      "id": "dep1",
      "name": "Engineering"
    }
  ]
}
```

---

# Department Module

## Create Department

Creates a new department.

### Endpoint

```http
POST /api/departments
```

### Request

```json
{
  "name": "Engineering"
}
```

### Response

```json
{
  "id": "dep1",
  "name": "Engineering"
}
```

---

## List Departments

Returns all departments.

### Endpoint

```http
GET /api/departments
```

### Response

```json
[
  {
    "id": "dep1",
    "name": "Engineering"
  }
]
```

---

# User Module

## Create User

Creates a new employee or administrator.

### Endpoint

```http
POST /api/users
```

### Request

```json
{
  "name": "Ajay",
  "email": "ajay@company.com",
  "password": "password123",
  "role": "EMPLOYEE",
  "departmentIds": [
    "dep1"
  ]
}
```

### Response

```json
{
  "id": "user-id"
}
```

---

## Bulk Import Users

Imports users through an Excel file.

### Endpoint

```http
POST /api/users/import
```

### Request

Multipart Form Data

```text
employees.xlsx
```

### Response

```json
{
  "success": 120,
  "failed": 2
}
```

---

## List Users

Returns paginated users.

### Endpoint

```http
GET /api/users?page=1&limit=20
```

### Response

```json
{
  "items": [],
  "total": 100
}
```

---

# Document Module

## Upload Document

Uploads a document and starts asynchronous processing.

### Endpoint

```http
POST /api/documents
```

### Request

Multipart Form Data

```text
file.pdf
departmentId=dep1
```

### Backend Flow

```text
Upload File
↓
Create Document Record
↓
Create Queue Job
↓
Return Success
```

### Response

```json
{
  "documentId": "doc123",
  "status": "PENDING"
}
```

---

## List Documents

Returns uploaded documents.

### Endpoint

```http
GET /api/documents
```

### Response

```json
[
  {
    "id": "doc123",
    "title": "Leave Policy",
    "status": "COMPLETED",
    "totalChunks": 120
  }
]
```

---

## Document Details

Returns metadata for a single document.

### Endpoint

```http
GET /api/documents/:id
```

### Response

```json
{
  "id": "doc123",
  "title": "Leave Policy",
  "url": "https://...",
  "status": "COMPLETED",
  "totalChunks": 120
}
```

---

# Chat Module

## Ask Question

Main RAG endpoint.

### Endpoint

```http
POST /api/chat
```

### Request

```json
{
  "question": "How do I request annual leave?"
}
```

### Backend Flow

```text
Authenticate User
↓
Get User Departments
↓
Generate Query Embedding
↓
Qdrant Vector Search
↓
BM25 Search
↓
Merge Results
↓
Rerank Results
↓
Select Top Chunks
↓
Generate Prompt
↓
OpenAI
↓
Build Citations
↓
Return Response
```

### Response

```json
{
  "answer": "Employees can request annual leave through the HR portal.",
  "sources": [
    {
      "documentId": "doc123",
      "title": "Leave Policy",
      "pageNumber": 5,
      "url": "https://..."
    }
  ]
}
```

---

# Administration Module

## Platform Metrics

Returns operational metrics.

### Endpoint

```http
GET /api/admin/metrics
```

### Response

```json
{
  "documentsIndexed": 1000000,
  "queriesToday": 15000,
  "cacheHitRate": 82,
  "averageLatencyMs": 750
}
```

---

# Common Response Format

Successful responses should follow:

```json
{
  "success": true,
  "data": {}
}
```

Failed responses should follow:

```json
{
  "success": false,
  "message": "Validation failed"
}
```

---

# API Design Principles

* REST-based APIs
* JWT Authentication
* Pagination for list endpoints
* Asynchronous document processing
* Consistent response structure
* Department-based authorization
* Source-backed AI responses
* Backward-compatible API evolution

```
```
