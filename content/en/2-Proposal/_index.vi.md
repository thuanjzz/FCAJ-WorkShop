---
title: "Proposal"
date: 2026-05-17
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Smart Media Analytics CloudForge
## Nền tảng Tìm kiếm Ngữ nghĩa Video bằng AI trên AWS

---

### 1. Tóm tắt dự án

**Smart Media Analytics CloudForge** là nền tảng cloud-native giúp designer và video editor tìm kiếm nội dung bên trong video bằng ngôn ngữ tự nhiên. Thay vì xem lại thủ công từng giây footage, người dùng chỉ cần upload video và hệ thống tự động trích xuất cảnh, chuyển lời nói thành văn bản, tạo mô tả AI cho keyframe và lưu semantic embeddings — giúp mọi giây video đều có thể tìm kiếm được.

MVP hướng đến nhóm sáng tạo nhỏ (5–20 người) và được xây dựng hoàn toàn trên AWS, thiết kế để scale từ prototype lên production mà không thay đổi hạ tầng.

**Tài nguyên dự án:**
- GitHub: https://github.com/ntnhan19/smart_media_analytics_cloudforge
- Figma Design: https://www.figma.com/design/sCSnjyJ8bTRwSKocFe0nAR/Viualization-Of-SMA

---

### 2. Bài toán

#### Vấn đề là gì?

Nội dung video tăng theo cấp số nhân nhưng vẫn **không thể tìm kiếm** được. Các nhóm sáng tạo làm việc với thư viện video lớn gặp phải:

- **Scrub thủ công:** Tìm một cảnh cụ thể phải xem footage từng frame.
- **Không có metadata:** File video thô không mang thông tin ngữ nghĩa về nội dung.
- **Kiến thức bị mất:** Lời nói, đối tượng và hành động bên trong video vô hình với công cụ tìm kiếm.
- **Workflow chậm:** Mỗi lần review 10 phút để tìm clip 5 giây tốn hàng giờ mỗi tuần.

#### Giải pháp

Pipeline AI 4 giai đoạn biến video thô thành kho kiến thức có thể tìm kiếm:

1. **Scene Detection** — Chia video thành cảnh có nghĩa (PySceneDetect + OpenCV / AWS MediaConvert + ECS Fargate).
2. **Speech-to-Text** — Chuyển lời nói thành văn bản có timestamp (faster-whisper / Amazon Transcribe).
3. **Vision Captioning** — Mô tả chi tiết từng keyframe (Ollama qwen2.5-vl:3b / AWS Bedrock Nova Lite).
4. **Semantic Embedding** — Chuyển mô tả văn bản thành vector 1024 chiều để tìm kiếm cosine similarity (bge-m3 / Bedrock Titan Embeddings v2).

Người dùng truy vấn bằng ngôn ngữ tự nhiên (VD: *"người viết bảng trắng"* hoặc *"outdoor scene at sunset"*) và nhận danh sách cảnh phù hợp với timestamp video trực tiếp.

---

### 3. Kiến trúc giải pháp

Hệ thống theo thiết kế **hybrid local-to-cloud**, trong đó stack Docker Compose local ánh xạ 1-to-1 với kiến trúc AWS production, cho phép migrate liền mạch.

#### Tổng quan kiến trúc AWS

```
User (Designer / Video Editor)
  │
  ├─ 1. Truy cập       → Route 53 → WAF → CloudFront → S3 (Frontend)
  ├─ 2. Xác thực       → Cognito + Secrets Manager
  ├─ 3. Gửi request    → API Gateway → App Runner (FastAPI Backend) [JWT]
  ├─ 4. Upload bảo mật → S3 (Media Storage & Keyframes)
  │
  ├─ 5. Sự kiện        → SQS Ingest Queue
  ├─ 6. Chuyển tiếp    → Amazon EventBridge
  ├─ 7. Điều phối      → AWS Step Functions
  │
  ├─ 8. Scene Splitter → ECS Fargate Task (PySceneDetect)
  │       ├─ Đọc từ S3 qua S3 Gateway Endpoint
  │       ├─ Lưu keyframes lên S3
  │       └─ Publish tiến độ → ElastiCache Redis
  │
  ├─ 9. Dịch vụ AI     → Bedrock Nova Lite (Captioning)
  │                    → Amazon Transcribe (ASR)
  │                    → Bedrock Titan Embeddings v2
  │
  ├─ 10. Lưu trữ       → AWS Lambda (DB Writer)
  │                    → RDS PostgreSQL + pgvector
  │
  └─ 11. Tìm kiếm      → Bedrock Titan Embeddings (query vector)
                       → RDS pgvector cosine similarity
```

**CI/CD:** GitHub Actions → Build & Push → Amazon ECR → ECS Fargate  
**Observability:** AWS X-Ray + CloudWatch Logs, Metrics, Alarms

#### Mapping Local ↔ AWS

| Thành phần | Local | AWS Cloud |
|---|---|---|
| Frontend | React 19 + Vite + Tailwind CSS | S3 + CloudFront |
| Backend API | FastAPI + Docker | App Runner |
| Object Storage | MinIO | S3 Standard + Lifecycle |
| Xử lý video | FFmpeg + PySceneDetect | MediaConvert + ECS Fargate |
| ASR | faster-whisper (int8) | Amazon Transcribe |
| Vision AI | Ollama qwen2.5-vl:3b | Bedrock Nova Lite |
| Embedding | Ollama bge-m3 (1024-dim) | Bedrock Titan Embeddings v2 |
| Vector Store | pgvector (local) | RDS PostgreSQL + pgvector |
| Relational DB | PostgreSQL 16 | RDS PostgreSQL (Multi-AZ) |
| Cache / Queue | Redis + BackgroundTasks | ElastiCache + SQS + Step Functions |

---

### 4. Triển khai kỹ thuật

#### Model Registry AI

| Thành phần | Model Local | AWS Equivalent |
|---|---|---|
| Scene Detection | PySceneDetect + OpenCV | MediaConvert + ECS Task |
| ASR | faster-whisper-base (int8) | Amazon Transcribe |
| Vision Captioning | qwen2.5-vl:3b (Q4_K_M) | Bedrock Amazon Nova Lite |
| Text Embedding | bge-m3 (1024-dim) | Bedrock Titan Embeddings v2 |
| LLM Refinement | qwen2:1.5b (Q4) | — |

#### Quyết định kỹ thuật chính

- **pgvector thay ChromaDB:** Loại bỏ dịch vụ vector DB riêng; lưu vector 1024-dim trực tiếp trong PostgreSQL — đơn giản hóa hạ tầng và cho phép JOIN giữa metadata và vectors.
- **App Runner thay Lambda:** FastAPI với WebSocket và AI processing chạy lâu cần container liên tục, không phải function invocation.
- **Exponential Backoff cho Bedrock:** Bedrock giới hạn mặc định 100 requests/phút; implement adaptive retry với `botocore.config` (Issue #72).
- **AI Provider Abstraction:** Pattern interface cho phép chuyển đổi giữa Ollama local và AWS Bedrock qua config — không cần thay đổi code pipeline.

---

### 5. Timeline & Milestones

| Giai đoạn | Thời gian | Milestone |
|---|---|---|
| **Nền tảng** | Tuần 1–4 | AWS fundamentals: EC2, S3, Lambda, VPC, DynamoDB, API Gateway |
| **Sprint 1** | Tuần 5–6 | Docker Compose stack, FastAPI backend, AI pipeline prototype |
| **Sprint 2** | Tuần 7–8 | Full AI pipeline, ChromaDB → pgvector, Figma UI design |
| **Sprint 3** | Tuần 9–10 | AWS Bedrock integration, Asset Detail Page, WebSocket real-time |
| **Sprint 4** | Tuần 11 | AWS Fargate deployment, E2E finalization, clean architecture |

---

### 6. Ước tính chi phí

| Dịch vụ AWS | Chi phí ước tính |
|---|---|
| S3 (Storage + Requests) | ~$0.50–$2.00/tháng |
| App Runner (Backend) | ~$10–$15/tháng |
| RDS PostgreSQL (db.t4g.micro) | ~$15–$30/tháng |
| Bedrock Nova Lite (Captioning) | ~$0.00008/ảnh (~$0.05 per 600 keyframes) |
| Bedrock Titan Embeddings v2 | ~$0.0001/1k tokens |
| Amazon Transcribe | ~$0.024/phút audio |
| ECS Fargate (mỗi lần chạy pipeline) | ~$0.01–$0.05/video |
| CloudFront + Route 53 | ~$0.50–$1.00/tháng |
| SQS + Step Functions | ~$0 (Free Tier) |
| **Tổng cộng (MVP, traffic thấp)** | **~$30–$55/tháng** |

---

### 7. Đánh giá rủi ro

| Rủi ro | Tác động | Xác suất | Giảm thiểu |
|---|---|---|---|
| Bedrock Throttling (HTTP 429) | Cao | Trung bình | Exponential backoff với `botocore.config`; retry decorator `tenacity` |
| Network Timeout (502/503/504) | Trung bình | Thấp | `mode='adaptive'`; ECS Task auto-retry qua Step Functions |
| Timeout xử lý file lớn | Cao | Trung bình | ECS Fargate Task timeout 30 phút; chunked processing |
| Latency vector search ở quy mô lớn | Trung bình | Thấp | pgvector HNSW index; giới hạn search trên subset asset liên quan |
| Vượt ngân sách | Thấp | Thấp | AWS Budget alerts $50; Lifecycle rules S3 Glacier |

---

### 8. Kết quả kỳ vọng

#### Sản phẩm bàn giao kỹ thuật
- Hệ thống Video Semantic Search hoạt động đầy đủ, truy cập qua trình duyệt.
- AI pipeline xử lý video trong dưới 2 phút với clip ≤ 2 phút.
- Tìm kiếm ngôn ngữ tự nhiên trả về kết quả cấp độ cảnh với timestamp video.
- Deploy đầy đủ trên AWS với CI/CD pipeline (GitHub Actions → ECR → ECS Fargate).

#### Kết quả học tập
- Kinh nghiệm end-to-end xây dựng hệ thống AI production trên AWS.
- Thực hành chuyên sâu: AWS Bedrock, ECS Fargate, RDS pgvector, App Runner.
- Hiểu tối ưu chi phí cloud, observability và clean architecture patterns.