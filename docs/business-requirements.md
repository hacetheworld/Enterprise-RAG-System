# Business Requirements

## Overview

Organizations often store knowledge across multiple internal systems such as document repositories, shared drives, internal wikis, and manually uploaded files.

As the volume of documentation grows, employees spend significant time searching for information across different sources.

The goal of this project is to build a centralized AI-powered knowledge retrieval platform that enables employees to quickly find accurate information from authorized company documentation.

---

## Actors

### Admin

Responsible for:

* Managing departments
* Managing employees
* Importing employees through Excel files
* Registering knowledge sources
* Uploading documents manually
* Monitoring document processing status

### Employee

Responsible for:

* Logging into the platform
* Asking questions in natural language
* Viewing AI-generated answers
* Viewing answer citations
* Opening referenced source documents

---

## Knowledge Sources

The system supports two types of knowledge sources.

### Manual Upload

Admins can:

* Upload PDF, DOCX, TXT and other supported files
* Assign the uploaded content to a department

### External Knowledge Sources

Admins can register external repositories such as:

* Confluence spaces
* SharePoint locations
* Google Drive folders
* Internal documentation portals

Each source must be associated with a department.

Documents retrieved from a source automatically inherit that department's access permissions.

---

## Access Control

Employees belong to a department.

Examples:

* HR
* Finance
* Engineering
* Operations

Employees can only access information originating from sources assigned to their department.

The system must prevent retrieval of unauthorized information.

---

## Knowledge Retrieval

Employees can ask questions using natural language.

The system should:

* Retrieve relevant information
* Generate grounded answers
* Provide source references
* Allow employees to open the original source document

---

## Non-Functional Requirements

* Support indexing of up to 1 million documents
* Fast query response times
* High retrieval accuracy
* Low hallucination rate
* Department-level access control
* Scalable ingestion pipeline
* Cost-efficient operation
* Production-ready observability
