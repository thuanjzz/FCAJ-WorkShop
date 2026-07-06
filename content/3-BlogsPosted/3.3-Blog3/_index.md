---
title: "Blog 3"
date: 2026-07-05
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# From ChromaDB to pgvector: Simplifying the AI Stack

### 1. Introduction

In early iterations of our Video Semantic Search project, we deployed **ChromaDB** as our standalone vector database. While ChromaDB is excellent for prototyping, managing a separate database instance just for vectors added unnecessary complexity to our infrastructure, deployment, and backups.

During Week 7, we made an architectural decision to migrate to **pgvector** — a PostgreSQL extension that allows us to store and query vector embeddings directly inside our primary relational database. This blog post covers the reasoning, migration steps, and query optimization using pgvector.

---

### 2. Why pgvector? (Architectural Trade-offs)

| Criteria | ChromaDB (Standalone) | pgvector (PostgreSQL Extension) |
|---|---|---|
| **Infrastructure** | Requires a separate service/port | Runs inside existing RDS PostgreSQL |
| **Data Consistency** | Hard to maintain ACID transactions between SQL metadata and vector DB | Full ACID compliance, native relational JOINs in a single query |
| **Backup & Restore** | Backup two separate systems | Single backup file (pg_dump) for both relational and vector data |
| **Learning Curve** | ChromaDB client API | Standard SQL queries |

---

### 3. Setting Up pgvector

#### Step 1: Enable the Extension
Once PostgreSQL is running, connect and run:
```sql
CREATE EXTENSION IF NOT EXISTS vector;
```

#### Step 2: Define the Table Schema
In our database, each video scene has a detailed AI caption. We convert this caption into a 1024-dimension vector embedding (using Bedrock Titan Embeddings v2). We store this in the `scenes` table:

```sql
CREATE TABLE scenes (
    id SERIAL PRIMARY KEY,
    asset_id INTEGER REFERENCES assets(id) ON DELETE CASCADE,
    start_time DOUBLE PRECISION,
    end_time DOUBLE PRECISION,
    caption TEXT,
    embedding vector(1024) -- Store 1024-dimension embeddings
);
```

---

### 4. Performing Semantic Search with Cosine Similarity

With pgvector, finding the most relevant scenes for a user query is simple. First, we generate the embedding vector of the search query (`[0.123, -0.456, ...]`), then we run a single SQL query:

```sql
SELECT id, start_time, end_time, caption, 
       1 - (embedding <=> '[0.123, -0.456, ...]') AS similarity
FROM scenes
WHERE asset_id = 4
ORDER BY embedding <=> '[0.123, -0.456, ...]'
LIMIT 5;
```

- `<=>` is the cosine distance operator in pgvector.
- `1 - (embedding <=> query)` converts cosine distance into **cosine similarity** (closer to 1 is more similar).
- By adding `WHERE asset_id = 4`, we instantly limit the search scope to a specific video asset, which is faster and cleaner than implementing metadata filtering in ChromaDB.

---

### 5. Optimizing Performance with HNSW Index

As the database grows to hundreds of thousands of scenes, sequential scanning becomes slow. We optimize queries by creating an **HNSW (Hierarchical Navigable Small World)** index:

```sql
CREATE INDEX ON scenes 
USING hnsw (embedding vector_cosine_ops) 
WITH (m = 16, ef_construction = 64);
```

This index builds a multi-layered graph for approximate nearest neighbor (ANN) search, reducing query latency from milliseconds to microseconds.

---

### 6. Conclusion

Migrating from ChromaDB to pgvector simplified our stack dramatically. We reduced our Docker container count, unified our backup strategy, and enabled powerful relational queries combined with semantic search. For teams already using PostgreSQL, pgvector is a no-brainer.