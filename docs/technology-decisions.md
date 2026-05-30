# Technology Decisions

## Repository Strategy

### Monorepo

Structure:

* Frontend application
* Backend application
* Shared packages
* Infrastructure configuration
* Documentation

Reason:

* Simplified development workflow
* Easier code sharing
* Centralized project management

---

## Frontend

### React

Reason:

* Component-based architecture
* Large ecosystem
* Fast development experience

---

## Backend

### Fastify + TypeScript

Reason:

* High performance
* Type safety
* Excellent API development experience

---

## Relational Database

### PostgreSQL

Used for:

* Employees
* Departments
* Knowledge sources
* Document metadata
* Processing status

Reason:

* Reliable relational data storage
* Mature ecosystem
* Strong consistency guarantees

---

## Vector Database

### Qdrant

Used for:

* Chunk embeddings
* Similarity search
* Metadata filtering

Reason:

* Efficient vector search
* Supports large-scale retrieval workloads
* Strong metadata filtering capabilities

---

## Cache

### Redis

Used for:

* Frequently requested answers
* Session-related caching
* Performance optimization

Reason:

* Reduces latency
* Reduces LLM costs
* Improves user experience

---

## Queue System

### BullMQ

Used for:

* Document ingestion
* Chunking
* Embedding generation
* Background processing

Reason:

* Prevents long-running jobs from blocking APIs
* Improves system reliability

---

## Search Strategy

### Hybrid Retrieval

Components:

* Vector Search (Qdrant)
* BM25 Keyword Search

Reason:

* Handles both semantic and exact-match queries
* Improves retrieval quality

---

## Reranking

Used to:

* Reorder retrieved results
* Improve context quality before generation

Reason:

* Higher answer accuracy
* Better grounding

---

## AI Model

### OpenAI

Used for:

* Answer generation
* Retrieval-augmented generation (RAG)

Reason:

* Fast implementation
* High-quality responses
* Strong production reliability

---

## Storage Strategy

### Manual Uploads

Original documents are stored in object storage.

### External Sources

Original documents remain in their source system.

The platform stores metadata and references required for retrieval and citation generation.

Reason:

* Avoids unnecessary duplication
* Supports source document references

---

## Deployment

### Docker

Reason:

* Consistent environments
* Easier deployment
* Simplified local development

---

## Observability

Used for:

* Logs
* Metrics
* System monitoring

Reason:

* Faster troubleshooting
* Production visibility
* Operational reliability
