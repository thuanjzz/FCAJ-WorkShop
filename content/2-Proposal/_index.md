---
title: "Proposal"
date: 2026-05-17
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Smart Media Analytics
## Building an AI-Powered Video Analysis and Search System (Video Semantic Search)

### 1. Project Overview

**Smart Media Analytics (SMA)** is a local-first system that automates media ingestion, scene segmentation, transcription, and semantic search. Users simply type a natural language query (e.g., "sunset on the beach") to locate the exact video timestamp, eliminating time-consuming manual playback. Initially, the system runs 100% internally via Docker for security and cost efficiency during testing, with a clear upgrade path to AWS (S3, Bedrock, RDS) when needed.

---

### 2. Problem Statement

**Current challenge:** Scattered media libraries with meaningless file names make manual footage searching extremely time-consuming. Processing everything through Cloud AI from the start is costly and risks data leakage.

**Solution:** SMA addresses this with an automated media processing pipeline. The system uses a React dashboard for asset browsing, FastAPI, ChromaDB, PostgreSQL, MinIO, and local AI models (Ollama, faster-whisper). When deployed on AWS, these components map to a production architecture using Amazon S3, AWS Bedrock, Amazon Transcribe, Amazon RDS PostgreSQL, orchestrated by Step Functions and ECS Fargate, with Frontend managed via AWS Amplify.

**Benefits and ROI:** The solution significantly reduces time spent finding footage, reading transcripts, and identifying needed scenes. Users type natural language queries and receive matching video segments instead of manually reviewing entire files. The local-first architecture reduces upfront costs, while the AWS mapping ensures long-term scalability.

---

### 3. Solution Architecture

SMA is designed as a hybrid local-to-cloud system. In the local phase, all workflows run within Docker Compose. When moving to AWS, the architecture leverages serverless services.

![System Architecture](/images/2-Proposal/platform_architecture.png)

#### AWS Services Utilized

**Networking & Delivery:**
- Amazon Route 53 (DNS)
- AWS Amplify (Frontend)
- Amazon VPC (with NAT Gateway)

**Security:**
- Amazon Cognito (JWT Authentication)
- AWS Secrets Manager (Key Management)

**Storage & Database:**
- Amazon S3 (Media Storage)
- Amazon RDS PostgreSQL (Metadata)
- Amazon ElastiCache (Redis Cache)

**Compute & API:**
- AWS App Runner (FastAPI Backend)
- Amazon API Gateway (Routing)
- Amazon ECS Fargate (Video Processing)

**AI/ML:**
- Amazon Bedrock (Embeddings)
- Amazon Transcribe (Speech-to-Text)

**Orchestration & Integration:**
- Amazon SQS, EventBridge (Queue & Event)
- AWS Step Functions (Workflow Orchestration)

**DevOps & Monitoring:**
- Amazon ECR (Docker Registry)
- Amazon CloudWatch & AWS X-Ray (Logs & Tracing)

#### Component Design

- **Frontend Layer:** React application managed and distributed via AWS Amplify combined with Route 53, authenticated by Cognito.
- **Application Layer (API):** Receives requests from API Gateway and routes to App Runner.
- **Processing Layer (Pipeline):** Step Functions invoke ECS (Fargate) tasks for video processing, then Bedrock & Transcribe extract semantic meaning.
- **Data Layer:** RDS stores metadata, S3 stores static files, ElastiCache handles caching. All reside in a secure VPC with NAT Gateway.

---

### 4. Technical Implementation

**Implementation Phases:**

- **Architecture Research:** Design data flows compatible with the AWS service list (1 month pre-internship).
- **Local-first Build:** Complete the internal pipeline with Docker Compose (Month 1).
- **Cloud-ready Standardization:** Begin replacing containers with AWS Services such as App Runner, Bedrock, RDS (Month 2).
- **Deployment & Testing:** Use CI/CD to build and push to ECR, configure CloudWatch monitoring, and launch the system (Month 3).

**Technical Requirements:**

- **Media Processing:** Scene segmentation using FFMPEG running in containerized environments.
- **Cloud Systems:** Proficiency in AWS Amplify, ECS Fargate, Step Functions, Bedrock, and RDS. AWS SDK for programmatic service integration.

---

### 5. Timeline & Milestones

| Phase | Period | Milestone |
|---|---|---|
| Pre-Internship | Month 0 | Architecture research and planning (1 month) |
| Month 1 | Week 1–4 | Build local-first workflow, test AI models |
| Month 2 | Week 5–8 | Design and migrate storage/database components (S3, RDS) to AWS |
| Month 3 | Week 9–12 | Complete Step Functions, test full pipeline flow, and Launch |
| Post-Launch | Up to 1 year | AI fine-tuning and system optimization |

---

### 6. Budget Estimation

With a production architecture using over 20 AWS services, the estimated baseline monthly cost for a small-to-medium deployment is as follows:

| AWS Service | Usage Purpose | Estimated Monthly Cost (USD) |
|---|---|---|
| Amazon RDS (PostgreSQL) | Main Database (Multi-AZ, db.t4g.small - db.t4g.micro, 20GB) | $25.00 - $45.00 |
| AWS NAT Gateway | Private Subnet Internet access (1 NAT, 24/7) | $32.40 |
| Amazon ElastiCache | Caching (Multi-AZ, cache.t4g.small, 2 nodes) | $32.00 |
| Amazon ECS (Fargate) | Auto-scaling video processing tasks (~1 vCPU, 2GB RAM) | $15.00 |
| Amazon Bedrock & Transcribe | AI processing, Speech-to-Text, Embeddings | $15.00 |
| AWS App Runner | Web Service / API (minimum 1 instance) | $7.00 |
| Amazon CloudWatch & X-Ray | Logs, Metrics, Alarms, Distributed Tracing | $5.00 |
| Amazon S3 | Media & keyframe storage (estimated ~50GB) | $2.00 |
| AWS Secrets Manager | Key and credential management (5 secrets) | $2.00 |
| Amazon API Gateway | REST API & WSS Push (by request volume) | $1.00 |
| AWS Amplify | Frontend hosting & CDN delivery | $1.00 |
| Amazon SQS, EventBridge, Step Functions | Workflow orchestration, event bus, queues | $1.00 |
| Amazon Route 53 | DNS management (1 Hosted Zone) | $0.50 |
| Amazon Cognito | User authentication (JWT) | $0.00 (Free Tier) |
| **Total Estimated Cost** | **Entire System** | **~$181.90** |

---

### 7. Risk Assessment

#### Risk Matrix

- Large media volumes: High impact, medium probability.
- Slow local AI inference: Medium impact, high probability.
- Cloud cost scaling: High impact, low-to-medium probability.

#### Mitigation Strategies

- Limit ingestion by batch and optimize batch processing.
- Cache model results; switch fully to AWS Bedrock if too slow.
- Set AWS budget alerts and closely monitor logs/metrics.

#### Contingency Plans

- If pipeline ingestion fails, retain basic metadata to prevent losing the entire media library.
- Migrate only components that need scaling to the cloud, rather than the entire system from the start.

---

### 8. Expected Outcomes

**Technical Deliverables:**
SMA enables users to ingest media, automatically create semantic indexes, search using natural language, and jump directly to the correct video timestamps.

**Long-term Value:**
Well-suited for design and film production teams needing secure, efficient internal media management. Local-first architecture enables rapid development with a clear upgrade path to AWS when needed.