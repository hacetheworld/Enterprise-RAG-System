# Data Model

## Overview

The system stores business data in PostgreSQL and retrieval data in Qdrant.

### PostgreSQL

Responsible for:

- Users
- Departments
- User Department Mapping
- Documents

### Qdrant

Responsible for:

- Document Chunks
- Embeddings
- Retrieval Metadata

Qdrant is used only for retrieval operations and is not considered the source of truth.

---

# Entity Relationship Diagram

Departments
    │
    ├──────────────┐
    │              │
    ▼              ▼

User Departments  Documents
    │
    ▼

Users

---

# Users

Represents administrators and employees.

## Fields

| Field | Type |
|---------|---------|
| id | UUID |
| name | VARCHAR |
| email | VARCHAR |
| password | VARCHAR |
| role | ENUM |
| created_at | TIMESTAMP |
| updated_at | TIMESTAMP |

## Roles

- ADMIN
- EMPLOYEE

## Notes

Passwords are stored as secure hashes and never stored in plain text.

---

# Departments

Represents business departments.

Examples:

- HR
- Engineering
- Finance
- Operations

## Fields

| Field | Type |
|---------|---------|
| id | UUID |
| name | VARCHAR |
| created_at | TIMESTAMP |
| updated_at | TIMESTAMP |

---

# User Departments

Defines department access for users.

A user can belong to multiple departments.

Examples:

Ajay

- Engineering
- Platform

Rohit

- HR

## Fields

| Field | Type |
|---------|---------|
| user_id | UUID |
| department_id | UUID |

---

# Documents

Represents indexed documents available for retrieval.

Documents may originate from:

- Manual Uploads
- External Knowledge Repositories

## Fields

| Field | Type |
|---------|---------|
| id | UUID |
| title | VARCHAR |
| url | TEXT |
| file_type | VARCHAR |
| department_id | UUID |
| status | ENUM |
| total_chunks | INTEGER |
| created_at | TIMESTAMP |
| updated_at | TIMESTAMP |

## Status

- PENDING
- PROCESSING
- COMPLETED
- FAILED

## Purpose

Used to:

- Track ingestion progress
- Generate citations
- Link answers back to source documents
- Monitor indexing status

---

# Qdrant Chunk Model

Each document is split into multiple chunks before indexing.

## Chunk Payload

```json
{
  "chunk_id": "uuid",
  "document_id": "uuid",
  "department_id": "uuid",
  "chunk_index": 12,
  "page_number": 5,
  "text": "...",
  "embedding": []
}
```

## Stored Metadata

- document_id
- department_id
- chunk_index
- page_number

This metadata enables:

- Department filtering
- Citations
- Source tracing

---

# Access Control Model

Department access is enforced during retrieval.

Employee
↓
Assigned Departments
↓
Qdrant Metadata Filter
↓
Search Results

Only chunks belonging to authorized departments can be retrieved.

Example:

Employee Departments:

- Engineering
- Platform

Qdrant Filter:

department_id IN (Engineering, Platform)

---

# Citation Model

Every answer must include references.

The retrieval pipeline uses:

Chunk
↓
document_id
↓
Document Metadata

Citation Information:

- Document Title
- Document URL
- Page Number

Users can open the original document directly from the citation.

---

# Scalability Considerations

PostgreSQL stores business entities only.

Qdrant stores retrieval data.

This separation allows the platform to support:

- Millions of documents
- Hundreds of millions of chunks
- Fast retrieval operations

without overloading the relational database.