---
title: "Proposal"
date: 2026-05-17
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Smart Media Analytics CloudForge
## AI-Powered Video Semantic Search Platform on AWS

---

### 1. Executive Summary

**Smart Media Analytics CloudForge** is a cloud-native platform that enables designers and video editors to search inside video content using natural language queries. Instead of manually scrubbing through footage, users upload a video and the system automatically extracts scenes, transcribes speech, generates AI captions for keyframes, and stores semantic embeddings — making every second of video fully searchable.

The MVP targets small creative teams (5–20 users) and is built entirely on AWS serverless and managed services, designed to scale from prototype to production without infrastructure changes.

**Project resources:**
- GitHub: https://github.com/ntnhan19/smart_media_analytics_cloudforge
- Figma Design: https://www.figma.com/design/sCSnjyJ8bTRwSKocFe0nAR/Viualization-Of-SMA

---

### 2. Problem Statement

#### What's the Problem?

Video content is growing exponentially but remains largely **unsearchable**. Creative teams working with large video libraries face:

- **Manual scrubbing:** Finding a specific scene requires watching footage frame-by-frame.
- **No metadata:** Raw video files carry no semantic metadata about their content.
- **Lost knowledge:** Speech, objects, and actions inside videos are invisible to search engines.
- **Slow workflows:** A 10-minute review cycle to find a 5-second clip wastes hours per week.

#### The Solution

A four-stage AI pipeline that transforms raw video into a fully searchable knowledge base:

1. **Scene Detection** — Splits video into meaningful scenes using content-aware analysis (PySceneDetect + OpenCV / AWS MediaConvert + ECS Fargate).
2. **Speech-to-Text** — Transcribes spoken content with timestamps (faster-whisper / Amazon Transcribe).
3. **Vision Captioning** — Generates detailed descriptions of each keyframe (Ollama qwen2.5-vl:3b / AWS Bedrock Nova Lite).
4. **Semantic Embedding** — Converts text descriptions into 1024-dimension vectors for cosine similarity search (bge-m3 / Bedrock Titan Embeddings v2).

Users query with natural language (e.g., *"person writing on whiteboard"* or *"cảnh ngoài trời lúc hoàng hôn"*) and receive a ranked list of matching scenes with direct video timestamps.

---

### 3. Solution Architecture

The system follows a **hybrid local-to-cloud** design, where the local Docker Compose stack mirrors the AWS production architecture 1-to-1, enabling seamless migration.

#### AWS Architecture Overview

```
User (Designer / Video Editor)
  │
  ├─ 1. Access Site     → Route 53 → WAF → CloudFront → S3 (Frontend)
  ├─ 2. Authentication  → Cognito + Secrets Manager
  ├─ 3. Send Request    → API Gateway → App Runner (FastAPI Backend) [JWT]
  ├─ 4. Secure Upload   → S3 (Media Storage & Keyframes)
  │
  ├─ 5. Event           → SQS Ingest Queue
  ├─ 6. Forward         → Amazon EventBridge
  ├─ 7. Orchestrate     → AWS Step Functions
  │
  ├─ 8. Scene Splitter  → ECS Fargate Task (PySceneDetect)
  │       ├─ Read from S3 via S3 Gateway Endpoint
  │       ├─ Save keyframes to S3
  │       └─ Publish progress → ElastiCache Redis
  │
  ├─ 9. AI Services     → Bedrock Nova Lite (Captioning)
  │                     → Amazon Transcribe (ASR)
  │                     → Bedrock Titan Embeddings v2
  │
  ├─ 10. Persist        → AWS Lambda (DB Writer)
  │                     → RDS PostgreSQL + pgvector
  │
  └─ 11. Search         → Bedrock Titan Embeddings (query vector)
                        → RDS pgvector cosine similarity
```

**CI/CD:** GitHub Actions → Build & Push → Amazon ECR → ECS Fargate  
**Observability:** AWS X-Ray + CloudWatch Logs, Metrics, Alarms

#### Local ↔ AWS Service Mapping

| Component | Local | AWS Cloud |
|---|---|---|
| Frontend | React 19 + Vite + Tailwind CSS | S3 + CloudFront |
| Backend API | FastAPI + Docker | App Runner |
| Object Storage | MinIO | S3 Standard + Lifecycle |
| Video Processing | FFmpeg + PySceneDetect | MediaConvert + ECS Fargate |
| ASR | faster-whisper (int8) | Amazon Transcribe |
| Vision AI | Ollama qwen2.5-vl:3b | Bedrock Nova Lite |
| Embedding | Ollama bge-m3 (1024-dim) | Bedrock Titan Embeddings v2 |
| Vector Store | pgvector (local) | RDS PostgreSQL + pgvector |
| Relational DB | PostgreSQL 16 | RDS PostgreSQL (Multi-AZ) |
| Cache / Queue | Redis + BackgroundTasks | ElastiCache + SQS + Step Functions |

---

### 4. Technical Implementation

#### AI Model Registry

| Component | Local Model | AWS Equivalent |
|---|---|---|
| Scene Detection | PySceneDetect + OpenCV | MediaConvert + ECS Task |
| ASR | faster-whisper-base (int8) | Amazon Transcribe |
| Vision Captioning | qwen2.5-vl:3b (Q4_K_M) | Bedrock Amazon Nova Lite |
| Text Embedding | bge-m3 (1024-dim) | Bedrock Titan Embeddings v2 |
| LLM Refinement | qwen2:1.5b (Q4) | — |

#### Key Technical Decisions

- **pgvector over ChromaDB:** Eliminated a separate vector DB service; store 1024-dim vectors directly in PostgreSQL — simplifying infrastructure and enabling JOIN queries between metadata and vectors.
- **App Runner over Lambda:** FastAPI with WebSocket support and long-running AI processing requires persistent containers, not function invocations.
- **Exponential Backoff for Bedrock:** AWS Bedrock default limit is 100 requests/min; implemented adaptive retry with `botocore.config` (Issue #72).
- **AI Provider Abstraction:** Interface pattern allows switching between local Ollama and AWS Bedrock via config — zero code changes.

---

### 5. Timeline & Milestones

| Phase | Period | Milestone |
|---|---|---|
| **Foundation** | Week 1–4 | AWS fundamentals: EC2, S3, Lambda, VPC, DynamoDB, API Gateway |
| **Sprint 1** | Week 5–6 | Docker Compose stack, FastAPI backend, basic AI pipeline prototype |
| **Sprint 2** | Week 7–8 | Full AI pipeline, ChromaDB → pgvector, Figma UI design |
| **Sprint 3** | Week 9–10 | AWS Bedrock integration, Asset Detail Page, real-time WebSocket |
| **Sprint 4** | Week 11 | AWS Fargate deployment, E2E finalization, clean architecture |

---

### 6. Budget Estimation

| AWS Service | Estimated Cost |
|---|---|
| S3 (Storage + Requests) | ~$0.50–$2.00/month |
| App Runner (Backend) | ~$10–$15/month |
| RDS PostgreSQL (db.t4g.micro) | ~$15–$30/month |
| Bedrock Nova Lite (Captioning) | ~$0.00008/image (~$0.05 per 600 keyframes) |
| Bedrock Titan Embeddings v2 | ~$0.0001/1k tokens |
| Amazon Transcribe | ~$0.024/minute audio |
| ECS Fargate (per pipeline run) | ~$0.01–$0.05/video |
| CloudFront + Route 53 | ~$0.50–$1.00/month |
| SQS + Step Functions | ~$0 (Free Tier) |
| **Total (MVP, low traffic)** | **~$30–$55/month** |

---

### 7. Risk Assessment

| Risk | Impact | Probability | Mitigation |
|---|---|---|---|
| Bedrock Throttling (HTTP 429) | High | Medium | Exponential backoff with `botocore.config`; `tenacity` retry decorator |
| Network Timeout (502/503/504) | Medium | Low | `mode='adaptive'` retry; ECS Task auto-retry on Step Functions |
| Large file processing timeout | High | Medium | ECS Fargate Task timeout set to 30min; chunked processing |
| Vector search latency at scale | Medium | Low | pgvector HNSW index; limit search to relevant asset subset |
| Cost overrun | Low | Low | AWS Budget alerts at $50 threshold; Lifecycle rules for S3 Glacier |

---

### 8. Expected Outcomes

#### Technical Deliverables
- A working Video Semantic Search system accessible via web browser.
- AI pipeline processing videos in under 2 minutes for clips ≤ 2 minutes long.
- Natural language search returning scene-level results with video timestamps.
- Full deployment on AWS with CI/CD pipeline (GitHub Actions → ECR → ECS Fargate).

#### Learning Outcomes
- End-to-end experience building a production AI system on AWS.
- Hands-on expertise with AWS Bedrock, ECS Fargate, RDS pgvector, App Runner.
- Understanding of cloud cost optimization, observability, and clean architecture patterns.