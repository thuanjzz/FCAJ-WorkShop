---
title: "Blogs Posted"
date: 2026-07-05
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

During the internship at FCAJ, I wrote and published technical blog posts to the [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj) community to share knowledge and document learnings.

### [Blog 1 — VPC Endpoints: Secure Private Access to Amazon S3](3.1-Blog1/)

This blog explains the two types of VPC endpoints for S3 access — **Gateway Endpoint** (for EC2 within VPC) and **Interface Endpoint** (for on-premises via VPN/Direct Connect). It covers use cases, configuration steps, and how to validate that traffic stays within the AWS private network without traversing the public internet. This topic was directly applied during the FCAJ internship workshop on **Secure Hybrid Access to S3**.

### [Blog 2 — AWS Bedrock Throttling: Exponential Backoff Strategy for Production AI Systems](3.2-Blog2/)

This blog documents the **ThrottlingException** issue encountered when integrating AWS Bedrock (Amazon Nova Lite) into the AI pipeline during the internship. It explains the root cause (100 requests/minute default limit), the adaptive retry solution using `botocore.config`, and how to test it using `botocore.stub.Stubber`. The post references GitHub Issue #72 from the `smart_media_analytics_cloudforge` project. Useful for anyone building batch AI processing systems on Bedrock.

### [Blog 3 — From ChromaDB to pgvector: Simplifying the AI Stack](3.3-Blog3/)

This blog shares the architectural decision to migrate from **ChromaDB** (a standalone vector database) to **pgvector** (a PostgreSQL extension). It covers the motivation (eliminating a separate service, enabling JOINs between metadata and vectors), the migration steps, query performance comparison, and the HNSW index configuration for production-scale cosine similarity search. A practical guide for teams building semantic search systems on AWS RDS.