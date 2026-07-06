---
title: "Event 2"
date: 2026-06-20
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# AWS Community Day Vietnam 2026

### 1. Purpose of the Event

**AWS Community Day Vietnam 2026** is a large-scale community-led conference designed for cloud enthusiasts, developers, and system architects. The event aimed to:
- Share real-world production experiences and best practices in building cloud-native applications.
- Present deep dives into cutting-edge services like **AWS Bedrock (Generative AI)**, **ECS Fargate**, and **Serverless scaling**.
- Facilitate networking between university students, industry cloud professionals, and AWS Solutions Architects.

---

### 2. Key Highlights & Attended Sessions

#### Session 1: Building GenAI Apps at Scale with AWS Bedrock
- Explored rate limit quotas, model evaluation, and throughput tuning for Bedrock FMs.
- Discussed the mitigation of `ThrottlingException` (HTTP 429) using client-side adaptive throttling and backoff strategies — which we applied in our project (Issue #72).

#### Session 2: Production Containers on ECS Fargate
- Focused on optimizing Docker image sizes, security practices in task definitions, and leveraging VPC endpoints for private resource fetching.
- Shared strategies for orchestrating short-running ECS Fargate tasks using AWS Step Functions.

#### Session 3: Vector Databases on AWS RDS with pgvector
- Deep dive into using PostgreSQL + pgvector for semantic search.
- Compared IVFFlat and HNSW indexes, demonstrating how HNSW achieves lower query latency at scale.

---

### 3. Key Learnings & Technical Value Gained

- **Resilient AI Ingestion:** Learned that exponential backoff is mandatory when batching Bedrock API calls. The session reinforced our decision to use `tenacity` for rate limit recovery.
- **HNSW Indexing:** Gained a deep understanding of why HNSW is superior to sequential scan for embedding search. We implemented this in Week 11 to optimize pgvector performance.
- **Private Link Optimization:** Understood how gateway and interface endpoints inside the VPC reduce data processing charges when ECS tasks pull assets from Amazon S3.

---

### 4. Application to the Internship Project

This event took place during Week 9 of my internship. The knowledge gained from the Bedrock and pgvector sessions was directly applied to migrate our pipeline from ChromaDB to **RDS pgvector** and implement the **Exponential Backoff adaptive retry** for Bedrock Nova Lite captioning, dramatically improving our system's reliability and performance.
