---
title: "Week 3 Worklog"
date: 2026-05-04
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives (04/05 – 10/05/2026):

* Understand DynamoDB and NoSQL database concepts.
* Design DynamoDB tables with appropriate partition keys and sort keys.
* Practice CRUD operations using AWS SDK (Python boto3).

### Tasks Carried Out This Week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Mon | Study NoSQL concepts vs. relational DB; DynamoDB core concepts: tables, items, attributes, primary keys | 04/05/2026 | 04/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| Tue | DynamoDB data modeling: partition key, sort key, GSI (Global Secondary Index), LSI (Local Secondary Index) | 05/05/2026 | 05/05/2026 | https://docs.aws.amazon.com/dynamodb/ |
| Wed | Query vs. Scan operations: performance implications, filter expressions, ProjectionExpression | 06/05/2026 | 06/05/2026 | https://docs.aws.amazon.com/dynamodb/ |
| Thu | **Practice:** Design and create a DynamoDB table; perform CRUD with boto3 (Python); implement Query with GSI | 07/05/2026 | 07/05/2026 | https://docs.aws.amazon.com/sdk-for-python/ |
| Fri | DynamoDB Streams, TTL (Time-To-Live), DynamoDB Accelerator (DAX); backup and restore | 08/05/2026 | 08/05/2026 | https://docs.aws.amazon.com/dynamodb/ |
| Sat | Review and consolidate DynamoDB knowledge; compare with project's PostgreSQL + pgvector choice | 09/05/2026 | 09/05/2026 | |
| Sun | Consolidate Week 3: Evaluate DynamoDB performance and prepare database architecture documentation for Sprint 1 | 10/05/2026 | 10/05/2026 | |

### Week 3 Achievements:

* Understood fundamental differences between SQL (relational) and NoSQL databases and when to use each.
* Designed a DynamoDB table schema with partition key + sort key for efficient access patterns.
* Created Global Secondary Indexes (GSIs) to enable flexible query patterns without full table scans.
* Implemented a Python script using boto3 for full CRUD operations: PutItem, GetItem, UpdateItem, DeleteItem, Query, Scan.
* Understood DynamoDB Streams for change data capture and TTL for automatic item expiration.
* Analyzed trade-offs: DynamoDB (schemaless, auto-scale, high throughput) vs. PostgreSQL (relational, complex queries, pgvector for embeddings) — confirmed PostgreSQL + pgvector is the right choice for the project's semantic search use case.
