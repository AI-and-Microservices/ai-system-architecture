# File Service

## Overview
Handles file upload, storage, and retrieval through Minio (S3-compatible).

## Stack
- Language: NodeJS (Express)
- Storage: Minio

## Main APIs

| Method | Endpoint | Description            |
|:------:|:---------|:------------------------|
| POST   | /upload  | Upload a file            |
| GET    | /files/:id | Download file by ID    |

## Internal Design
- Files are uploaded to Minio.
- Metadata is stored optionally in MongoDB (for search and tagging).

---
