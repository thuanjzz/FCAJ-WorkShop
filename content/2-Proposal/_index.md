---
title: "Proposal"
date: 2026-05-17
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Smart Media Analytics
## AI-Powered Video Analysis and Search System (Video Semantic Search)

### 1. Executive Summary

Smart Media Analytics (SMA) is a local-first platform developed to automate the workflow of media ingestion, scene segmenting, transcription, and semantic search. The system allows users to enter natural language queries (such as "sunset scene on the beach") to pinpoint the exact timestamp in the video, eliminating time-consuming manual browsing and playback. To optimize testing costs and data security, the system is designed to run in a self-contained Docker environment during the initial phase, with a clear upgrade path prepared for migration to AWS (S3, Bedrock, OpenSearch, RDS) to support production scaling.

Project resources:
- GitHub: https://github.com/ntnhan19/smart_media_analytics_cloudforge
- Figma Design: https://www.figma.com/design/sCSnjyJ8bTRwSKocFe0nAR/Viualization-Of-SMA

---

### 2. Problem Statement

Current challenge: Digital media libraries are often distributed and raw video files carry meaningless names, making manual searching for specific footage labor-intensive. In addition, migrating all data directly to Cloud AI platforms from the start causes high operational costs and presents potential information security risks.

Proposed solution: SMA optimizes processing workflows using an automated pipeline running entirely in the local environment. The system integrates a React management dashboard, FastAPI to serve API endpoints, ChromaDB to manage vector embeddings, PostgreSQL for metadata storage, MinIO for static media object storage, Ollama to run vision and embedding models locally, and faster-whisper for speech-to-text processing. Upon cloud migration, these local services map to their cloud production counterparts on AWS: MinIO to Amazon S3, Ollama vision models to AWS Bedrock, faster-whisper to Amazon Transcribe, and PostgreSQL to Amazon RDS PostgreSQL. The entire cloud lifecycle is reinforced by management services such as CloudFront, Cognito, WAF, ECR, CloudWatch, Secrets Manager, X-Ray, SQS, EventBridge, Step Functions, API Gateway, App Runner, ECS Fargate, Lambda, ElastiCache, and VPC.

Delivered value: The system helps design and media production teams significantly reduce the time spent filtering raw footage, reviewing dialogs, and trimming videos. By typing natural commands like "sunset scene on the beach with wave sounds", users receive a ranked list of matched segments and direct timestamps, bypassing manual screening.

---

### 3. Solution Architecture

The architecture of SMA employs a hybrid local-to-cloud model to maintain flexibility during local development while minimizing deployment risks during migration to high-scale cloud environments. In the local phase, the entire data ingestion and analysis pipeline is containerized using Docker Compose: performing video reception, scene detection, audio transcription, automated visual description via AI, and local data persistence.

![System Architecture](/images/2-Proposal/platform_architecture.jpeg)

Upon transitioning to AWS, the system scales smoothly without restructuring core processing logic. Amazon S3 takes over media storage, AWS Bedrock and Amazon Transcribe execute machine learning tasks, and RDS PostgreSQL manages metadata. Frontend distribution is optimized via CloudFront and S3, supported by Cognito, WAF, ECR, Lambda, ECS Fargate, App Runner, Step Functions, SQS, EventBridge, CloudWatch, X-Ray, ElastiCache, and VPC to provide security, workflow orchestration, and system observability.

#### AWS Services Utilized in the Architecture

Based on the production design, the system deeply integrates specialized AWS services:

Networking & Delivery:
- Amazon Route 53: DNS domain management.
- Amazon CloudFront: CDN network to accelerate frontend and media delivery.
- Amazon VPC: Virtual private network with Internet Gateway and NAT Gateway to secure Private Subnet internet access.

Security & Identity:
- AWS WAF: Web application firewall protecting against cyber attacks.
- Amazon Cognito: User authentication and authorization (JWT token management).
- AWS Secrets Manager: Secure storage for API keys and database credentials.

Storage & Database:
- Amazon S3: Storage for raw media and keyframes (integrated via S3 Gateway Endpoint).
- Amazon RDS (PostgreSQL): Main relational database for metadata.
- Amazon ElastiCache (Redis): High-speed cache to reduce database load.

Compute & API:
- AWS App Runner: Simple, auto-scaling runner for FastAPI Backend.
- Amazon API Gateway: Communication gateway for REST API and WebSocket (WSS Push).
- Amazon ECS (Fargate): Auto-scaling tasks to process videos and images.
- AWS Lambda: Serverless code execution (e.g., writing metadata, triggering events).

Artificial Intelligence (AI/ML):
- Amazon Bedrock: Uses models like Nova Lite & Titan Embeddings to generate embeddings and analyze semantics.
- Amazon Transcribe: Speech-to-text conversion.

Integration & Orchestration:
- Amazon SQS & Amazon EventBridge: Message queues and event bus for internal event delivery.
- AWS Step Functions: Workflow orchestration for complex processing pipelines.

DevOps & Monitoring:
- Amazon ECR: Docker image registry for ECS and App Runner.
- Amazon CloudWatch & AWS X-Ray: Logs, metrics, alarms, and distributed tracing.

#### Core Component Design

- Frontend Layer: React application accessed via Route 53 + CloudFront, protected by WAF and authenticated via Cognito.
- Application Layer (API): Receives requests from API Gateway and routes them to App Runner (Backend).
- Processing Pipeline Layer: Uses Step Functions, EventBridge, and SQS to invoke ECS (Fargate) tasks for video processing, followed by Bedrock & Transcribe for semantic extraction.
- Data Layer: S3 for static files, RDS for metadata, ElastiCache for caching, and Lambda to assist writing data from the processing flow. The entire layer resides in a secure VPC with NAT Gateway. Secrets are managed centrally in Secrets Manager. CloudWatch and X-Ray monitor the entire application.

---

### 4. Technical Implementation

The project development workflow is structured across 4 sequential stages:
- Mapping layout: Model data transfer flows compatible with AWS platform definitions.
- Local setup: Establish and verify the internal pipeline via Docker Compose.
- Cloud-ready transition: Progressively replace local Docker containers with corresponding AWS cloud services (e.g., App Runner, Bedrock, RDS).
- AWS integration & testing: Set up CI/CD workflows to package and push Docker images to ECR, and configure logging and distributed tracing via CloudWatch and X-Ray.

#### Key Technical Decisions
- pgvector over ChromaDB: Eliminated a separate vector DB service; store 1024-dim vectors directly in PostgreSQL — simplifying infrastructure and enabling JOIN queries between metadata and vectors.
- App Runner over Lambda: FastAPI with WebSocket and long-running AI processing requires persistent containers, not function invocations.
- Exponential Backoff for Bedrock: AWS Bedrock default limit is 100 requests/min; implemented adaptive retry with botocore.config.
- AI Provider Abstraction: Interface pattern allows switching between local Ollama and AWS Bedrock via config — zero code changes.

---

### 5. Timeline & Milestones

| Phase | Period | Milestone |
|---|---|---|
| Foundation | Week 1–4 | AWS fundamentals: EC2, S3, Lambda, VPC, DynamoDB, API Gateway |
| Sprint 1 | Week 5–6 | Docker Compose stack, FastAPI backend, AI pipeline prototype |
| Sprint 2 | Week 7–8 | Full AI pipeline, ChromaDB → pgvector, Figma UI design |
| Sprint 3 | Week 9–10 | AWS Bedrock integration, Asset Detail Page, real-time WebSocket |
| Sprint 4 | Week 11 | AWS Fargate deployment, E2E finalization, clean architecture |

---

### 6. Cost Estimation (AWS Environment)

The production infrastructure is optimized on AWS with support services running 24/7. Below is the estimated baseline monthly cost for a small-to-medium deployment:

| AWS Service | Usage Purpose | Estimated Monthly Cost (USD) |
|---|---|---|
| Amazon RDS (PostgreSQL) | Main Database (Multi-AZ, db.t4g.medium, 20GB) | $68.00 |
| AWS NAT Gateway | Secures Private Subnet access to the Internet (1 NAT, 24/7) | $32.40 |
| Amazon ElastiCache | Caching (Multi-AZ, cache.t4g.micro, 2 nodes) | $32.00 |
| Amazon ECS (Fargate) | Runs auto-scaling tasks for video processing (~1 vCPU, 2GB RAM) | $15.00 |
| Amazon Bedrock & Transcribe | AI processing, Speech-to-Text, Embeddings (baseline estimate) | $15.00 |
| AWS App Runner | Web Service / API (minimum 1 instance) | $7.00 |
| AWS WAF | Web Application Firewall (1 Web ACL, 1 Rule) | $6.00 |
| Amazon CloudWatch & X-Ray | Logs, Metrics, Alarms, Distributed Tracing | $5.00 |
| Amazon S3 | Media & keyframe storage (estimated ~50GB) | $2.00 |
| AWS Secrets Manager | Manages keys and credentials (5 secrets) | $2.00 |
| Amazon API Gateway | REST API & WSS Push (by requests volume) | $1.00 |
| Amazon CloudFront | Content delivery network data transfer | $1.00 |
| Amazon SQS, EventBridge, Step Functions | Workflow orchestration, event bus, queues | $1.00 |
| Amazon Route 53 | DNS management (1 Hosted Zone) | $0.50 |
| Amazon Cognito | User authentication (JWT) | $0.00 (Free Tier) |
| AWS Lambda | Metadata & embedding persistence | $0.00 (Free Tier) |
| **Total Estimated Cost** | **Entire System** | **~$187.90** |

---

### 7. Risk Assessment

#### Risk Matrix
- Large media volumes: High impact, medium probability.
- Slow local AI inference: Medium impact, high probability.
- Search result inaccuracies: Medium impact, medium probability.
- Cloud cost scaling issues: High impact, low-to-medium probability.

#### Mitigation Strategies
- Implement sequential local pipelines, model caching, and optimized batch processing.
- Separate metadata, vector search, and object storage for easy migration to AWS.
- Restrict ingestion batch sizes and track performance using log metrics.
- Migrate only scaling components to the cloud rather than the entire system from day one.

#### Contingency Plans
- If local AI inference is too heavy, reduce model size or switch to Bedrock during cloud phase.
- If local vector search is slow, migrate to OpenSearch Serverless early.
- If pipeline ingestion fails, retain basic metadata to prevent losing the entire media library.

---

### 8. Expected Outcomes

- Technical Deliverables: SMA enables users to upload media, automatically create semantic indexes, search via natural language, and jump directly to the correct video timestamps.
- Product Outcomes: The system is well-suited for design, video editing, content research teams, or laboratories needing to manage internal media libraries securely and efficiently.
- Scalability Outcomes: Local-first architecture speeds up development, while AWS mapping provides a clear upgrade path to a production environment.