---
title: "Bản đề xuất"
date: 2026-05-17
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Smart Media Analytics
## Xây dựng hệ thống phân tích và tìm kiếm video bằng AI (Video Semantic Search)

### 1. Tổng quan dự án

**Smart Media Analytics (SMA)** là hệ thống local-first tự động nạp, tách cảnh, phiên âm và tìm kiếm ngữ nghĩa media. Người dùng chỉ cần gõ yêu cầu tự nhiên (ví dụ: "hoàng hôn trên biển") để tìm đúng timestamp video, tiết kiệm thời gian xem thủ công. Ban đầu, hệ thống chạy 100% nội bộ qua Docker giúp bảo mật và tối ưu chi phí thử nghiệm, sau đó có thể dễ dàng nâng cấp cấu trúc tương ứng lên AWS (S3, Bedrock, RDS) khi cần.

---

### 2. Vấn đề cần giải quyết

**Vấn đề hiện tại:** Thư viện media phân tán, tên file vô nghĩa khiến việc tìm cảnh quay thủ công tốn rất nhiều thời gian. Nếu xử lý toàn bộ qua AI Cloud ngay từ đầu sẽ tốn kém và có nguy cơ rò rỉ dữ liệu.

**Giải pháp:** SMA giải quyết bài toán này bằng một pipeline xử lý media tự động. Hệ thống sử dụng React dashboard để duyệt asset, FastAPI, ChromaDB, PostgreSQL, MinIO, cùng các mô hình AI nội bộ (Ollama, faster-whisper). Khi triển khai trên AWS, các thành phần này được ánh xạ sang kiến trúc production sử dụng Amazon S3, AWS Bedrock, Amazon Transcribe, Amazon RDS PostgreSQL, điều phối bởi Step Functions và ECS Fargate, và quản lý Frontend qua AWS Amplify.

**Lợi ích và ROI:** Giải pháp giúp rút ngắn đáng kể thời gian tìm kiếm footage, đọc transcript và xác định cảnh cần dùng. Người dùng có thể nhập truy vấn tự nhiên và nhận kết quả đúng đoạn video thay vì phải xem thủ công toàn bộ file. Kiến trúc local-first giúp tiết kiệm chi phí ban đầu, trong khi bản đồ lên AWS đảm bảo khả năng mở rộng lâu dài.

---

### 3. Kiến trúc giải pháp

SMA được thiết kế theo hướng hybrid local-to-cloud. Ở giai đoạn local, toàn bộ quy trình chạy trong Docker Compose. Khi lên AWS, kiến trúc tận dụng các dịch vụ serverless.

![Kiến trúc hệ thống](/images/2-Proposal/platform_architecture.png)

#### Danh sách dịch vụ AWS sử dụng

**Mạng & Phân phối:**
- Amazon Route 53 (DNS)
- AWS Amplify (Frontend)
- Amazon VPC (kèm NAT Gateway)

**Bảo mật:**
- Amazon Cognito (Xác thực JWT)
- AWS Secrets Manager (Quản lý keys)

**Lưu trữ & Database:**
- Amazon S3 (Lưu trữ Media)
- Amazon RDS PostgreSQL (Metadata)
- Amazon ElastiCache (Redis Cache)

**Tính toán & API:**
- AWS App Runner (FastAPI Backend)
- Amazon API Gateway (Routing)
- Amazon ECS Fargate (Xử lý video)

**AI/ML:**
- Amazon Bedrock (Embeddings)
- Amazon Transcribe (Speech-to-Text)

**Điều phối & Tích hợp:**
- Amazon SQS, EventBridge (Hàng đợi & Event)
- AWS Step Functions (Điều phối Workflow)

**DevOps & Giám sát:**
- Amazon ECR (Docker Registry)
- Amazon CloudWatch & AWS X-Ray (Logs & Tracing)

#### Thiết kế thành phần

- **Lớp Giao diện (Frontend):** Ứng dụng React quản lý và phân phối qua AWS Amplify kết hợp Route 53, xác thực bằng Cognito.
- **Lớp Ứng dụng (API):** Nhận request từ API Gateway và định tuyến đến App Runner.
- **Lớp Xử lý (Pipeline):** Step Functions gọi các ECS (Fargate) tasks xử lý video, sau đó dùng Bedrock & Transcribe trích xuất ngữ nghĩa.
- **Lớp Dữ liệu (Data):** RDS lưu siêu dữ liệu, S3 lưu file tĩnh, ElastiCache lưu đệm. Tất cả nằm trong VPC bảo mật với NAT Gateway.

---

### 4. Triển khai kỹ thuật

**Các giai đoạn triển khai:**

- **Nghiên cứu kiến trúc:** Thiết kế luồng dữ liệu khớp với danh sách dịch vụ AWS (1 tháng trước thực tập).
- **Xây dựng bản Local-first:** Hoàn thiện pipeline nội bộ với Docker Compose (Tháng 1).
- **Chuẩn hóa Cloud-ready:** Bắt đầu thay thế các container bằng AWS Services như App Runner, Bedrock, RDS (Tháng 2).
- **Triển khai & Kiểm thử:** Sử dụng CI/CD để build đẩy lên ECR, cấu hình giám sát CloudWatch và ra mắt hệ thống (Tháng 3).

**Yêu cầu kỹ thuật:**

- **Xử lý Media:** Tách cảnh bằng FFMPEG chạy trong môi trường container.
- **Hệ thống Cloud:** Nắm vững AWS Amplify, ECS Fargate, Step Functions, Bedrock, và RDS. Sử dụng AWS SDK để lập trình tích hợp giữa các dịch vụ.

---

### 5. Timeline (Tiến độ)

| Giai đoạn | Thời gian | Milestone |
|---|---|---|
| Pre-Internship | Tháng 0 | Nghiên cứu và lên kế hoạch kiến trúc (1 tháng) |
| Tháng 1 | Tuần 1–4 | Xây dựng luồng local-first, kiểm thử các model AI |
| Tháng 2 | Tuần 5–8 | Thiết kế và đưa các thành phần lưu trữ, database (S3, RDS) lên AWS |
| Tháng 3 | Tuần 9–12 | Hoàn thiện Step Functions, kiểm thử toàn bộ luồng pipeline và Launch |
| Post-Launch | Tối đa 1 năm | Tinh chỉnh AI và tối ưu hóa hệ thống |

---

### 6. Ngân sách

Với quy mô kiến trúc production sử dụng hơn 20 dịch vụ AWS, ước tính chi phí cơ bản cho một môi trường nhỏ/vừa như sau:

| Dịch vụ AWS | Mục đích sử dụng | Ước tính chi phí / Tháng (USD) |
|---|---|---|
| Amazon RDS (PostgreSQL) | Database chính (Multi-AZ, db.t4g.small - db.t4g.micro, 20GB) | $25.00 - $45.00 |
| AWS NAT Gateway | Cho phép Private Subnet ra Internet (1 NAT, 24/7) | $32.40 |
| Amazon ElastiCache | Cache (Multi-AZ, cache.t4g.small, 2 nodes) | $32.00 |
| Amazon ECS (Fargate) | Chạy Auto Scaling Task xử lý Video (~1 vCPU, 2GB RAM) | $15.00 |
| Amazon Bedrock & Transcribe | Xử lý AI, Speech-to-Text, Embeddings | $15.00 |
| AWS App Runner | Web Service / API (1 instance tối thiểu) | $7.00 |
| Amazon CloudWatch & X-Ray | Logs, Metrics, Alarms, Distributed Tracing | $5.00 |
| Amazon S3 | Lưu trữ Media & Keyframes (Ước tính ~50GB) | $2.00 |
| AWS Secrets Manager | Quản lý Keys, Credentials (5 secrets) | $2.00 |
| Amazon API Gateway | REST API & WSS Push (Theo request) | $1.00 |
| AWS Amplify | Hosting & CDN phân phối Frontend | $1.00 |
| Amazon SQS, EventBridge, Step Functions | Điều phối Workflow, Event Bus, Hàng đợi | $1.00 |
| Amazon Route 53 | Quản lý DNS (1 Hosted Zone) | $0.50 |
| Amazon Cognito | Xác thực người dùng (JWT) | $0.00 (Free Tier) |
| **Tổng cộng ước tính** | **Toàn bộ hệ thống** | **~$181.90** |

---

### 7. Rủi ro

#### Ma trận rủi ro

- Khối lượng media lớn: Ảnh hưởng cao, xác suất trung bình.
- Mô hình AI chạy chậm trên máy local: Ảnh hưởng trung bình, xác suất cao.
- Chi phí cloud tăng khi scale: Ảnh hưởng cao, xác suất thấp đến trung bình.

#### Chiến lược giảm thiểu

- Giới hạn ingest theo đợt và tối ưu batch processing.
- Cache model và kết quả, nếu quá chậm sẽ chuyển hẳn sang AWS Bedrock.
- Đặt cảnh báo AWS budget và theo dõi sát log/metrics.

#### Kế hoạch dự phòng

- Nếu pipeline ingest lỗi, hệ thống vẫn giữ metadata cơ bản để tránh mất toàn bộ thư viện media.
- Chỉ đưa các thành phần cần scale lên cloud thay vì chuyển toàn bộ ngay từ đầu.

---

### 8. Kết quả kỳ vọng

**Cải tiến kỹ thuật:**
SMA cho phép người dùng nạp media, tự động tạo chỉ mục ngữ nghĩa, tìm kiếm bằng ngôn ngữ tự nhiên và nhảy trực tiếp tới đúng timestamp.

**Giá trị dài hạn:**
Phù hợp cho nhóm thiết kế, dựng phim cần quản lý media nội bộ an toàn và hiệu quả. Kiến trúc local-first giúp phát triển nhanh, kèm đường nâng cấp rõ ràng lên AWS khi cần.