---
title: "Bản đề xuất"
date: 2026-05-17
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Smart Media Analytics
## Xây dựng hệ thống phân tích và tìm kiếm video bằng AI (Video Semantic Search)

### 1. Tóm tắt điều hành

Smart Media Analytics (SMA) là hệ thống local-first tự động nạp, tách cảnh, phiên âm và tìm kiếm ngữ nghĩa media. Người dùng chỉ cần gõ yêu cầu tự nhiên (ví dụ: "hoàng hôn trên biển") để tìm đúng timestamp video, tiết kiệm thời gian xem thủ công. Ban đầu, hệ thống chạy 100% nội bộ qua Docker giúp bảo mật và tối ưu chi phí thử nghiệm, sau đó có thể dễ dàng nâng cấp cấu trúc tương ứng lên AWS (S3, Bedrock, OpenSearch, RDS) khi cần.

Tài nguyên dự án:
- GitHub: https://github.com/ntnhan19/smart_media_analytics_cloudforge
- Figma Design: https://www.figma.com/design/sCSnjyJ8bTRwSKocFe0nAR/Viualization-Of-SMA

---

### 2. Tuyên bố vấn đề

Vấn đề hiện tại: Thư viện media phân tán, tên file vô nghĩa khiến việc tìm cảnh quay thủ công tốn rất nhiều thời gian. Nếu xử lý toàn bộ qua AI Cloud ngay từ đầu sẽ tốn kém và có nguy cơ rò rỉ dữ liệu.

Giải pháp: SMA giải quyết bài toán này bằng một pipeline xử lý media tự động theo hướng local-first. Hệ thống sử dụng React dashboard để duyệt asset, FastAPI để cung cấp API, ChromaDB để lưu và truy vấn vector embedding, PostgreSQL để lưu metadata, MinIO để lưu object media, Ollama để chạy mô hình vision và embedding nội bộ, cùng faster-whisper để phiên âm âm thanh. Khi triển khai trên AWS, các thành phần này được ánh xạ sang kiến trúc production tương ứng: MinIO sang Amazon S3, Ollama vision model sang AWS Bedrock, faster-whisper sang Amazon Transcribe, và PostgreSQL sang Amazon RDS PostgreSQL. Bên cạnh đó, hệ thống cloud còn có thể dùng CloudFront, Cognito, WAF, ECR, CloudWatch, Secrets Manager, X-Ray, SQS, EventBridge, Step Functions, API Gateway, App Runner, ECS Fargate, Lambda, ElastiCache và VPC để hoàn thiện vòng đời triển khai và vận hành.

Lợi ích (ROI): Giải pháp giúp rút ngắn đáng kể thời gian tìm kiếm footage, đọc transcript và xác định cảnh cần dùng, từ đó tăng hiệu quả làm việc cho nhóm sáng tạo. Người dùng có thể nhập truy vấn tự nhiên như "cảnh hoàng hôn trên biển có tiếng sóng" và nhận kết quả đúng đoạn video, đúng timestamp, thay vì phải xem thủ công toàn bộ file.

---

### 3. Kiến trúc giải pháp

SMA được thiết kế theo hướng hybrid local-to-cloud để vừa thuận tiện khi phát triển nội bộ, vừa dễ nâng cấp lên môi trường production sau này. Ở giai đoạn local, toàn bộ quy trình ingest media chạy trong Docker Compose: hệ thống nhận video và ảnh, tự tách cảnh, phiên âm âm thanh, tạo mô tả nội dung bằng AI và lưu dữ liệu xử lý vào môi trường cục bộ.

Khi triển khai lên AWS, kiến trúc có thể mở rộng mà không cần thay đổi lớn về logic xử lý. S3 dùng để lưu media, Bedrock và Transcribe đảm nhiệm phần AI, RDS PostgreSQL lưu metadata, CloudFront kết hợp S3 phân phối giao diện frontend, còn các dịch vụ như Cognito, WAF, ECR, Lambda, ECS Fargate, App Runner, Step Functions, SQS, EventBridge, CloudWatch, X-Ray, ElastiCache và VPC hỗ trợ bảo mật, điều phối, giám sát và mở rộng hệ thống.

#### Danh sách dịch vụ AWS sử dụng trong kiến trúc

Dựa trên thiết kế production, hệ thống tích hợp sâu các dịch vụ chuyên biệt của AWS:

Mạng & Phân phối (Networking & Delivery):
- Amazon Route 53: Quản lý tên miền (DNS).
- Amazon CloudFront: Mạng phân phối nội dung (CDN) giúp tăng tốc độ tải Frontend và Media.
- Amazon VPC: Xây dựng mạng riêng ảo, bao gồm Internet Gateway và NAT Gateway để Private Subnet truy cập an toàn ra Internet.

Lớp Bảo mật (Security & Identity):
- AWS WAF: Tường lửa bảo vệ web khỏi các cuộc tấn công mạng.
- Amazon Cognito: Cung cấp xác thực và phân quyền người dùng (quản lý JWT token).
- AWS Secrets Manager: Lưu trữ an toàn các API Keys, database credentials.

Lưu trữ & Database (Storage & Database):
- Amazon S3: Lưu trữ toàn bộ media gốc, keyframes (tích hợp S3 Gateway Endpoint).
- Amazon RDS (PostgreSQL): Cơ sở dữ liệu quan hệ chính để lưu metadata.
- Amazon ElastiCache (Redis): Cache dữ liệu tốc độ cao để giảm tải DB.

Tính toán & Xử lý (Compute & API):
- AWS App Runner: Chạy Web Service / API Backend (FastAPI) một cách đơn giản, auto-scaling.
- Amazon API Gateway: Cổng giao tiếp cho REST API và WebSocket (WSS Push).
- Amazon ECS (Fargate): Triển khai các tác vụ Auto Scaling để xử lý video/hình ảnh.
- AWS Lambda: Thực thi mã serverless (như lưu metadata, trigger events).

Trí tuệ nhân tạo (AI/ML):
- Amazon Bedrock: Dùng các model như Nova Lite & Titan Embeddings để sinh embeddings và phân tích ngữ nghĩa.
- Amazon Transcribe: Nhận dạng và chuyển giọng nói thành văn bản (Speech-to-Text).

Tích hợp & Điều phối (Integration):
- Amazon SQS & Amazon EventBridge: Hàng đợi tin nhắn và Event bus để truyền tải sự kiện nội bộ.
- AWS Step Functions: Điều phối chuỗi quy trình làm việc (workflow) phức tạp (như pipeline xử lý video).

DevOps & Giám sát (Management & Governance):
- Amazon ECR: Lưu trữ các Docker images cho ECS, App Runner.
- Amazon CloudWatch & AWS X-Ray: Thu thập logs, metrics, alarms và giám sát phân tán (Distributed Tracing).

#### Thiết kế thành phần cốt lõi

- Lớp Giao diện (Frontend): Ứng dụng React truy cập qua Route 53 + CloudFront, được bảo vệ bằng WAF và xác thực bằng Cognito.
- Lớp Ứng dụng (API): Nhận request từ API Gateway và định tuyến đến App Runner (Backend).
- Lớp Xử lý (Processing Pipeline): Sử dụng Step Functions, EventBridge và SQS để gọi các ECS (Fargate) tasks xử lý video, sau đó dùng Bedrock & Transcribe để trích xuất ngữ nghĩa.
- Lớp Dữ liệu (Data): S3 lưu trữ file tĩnh, RDS lưu trữ siêu dữ liệu, ElastiCache lưu đệm và Lambda hỗ trợ đẩy dữ liệu từ luồng xử lý. Toàn bộ nằm trong VPC an toàn với NAT Gateway. Các khóa bảo mật quản lý tập trung ở Secrets Manager. CloudWatch và X-Ray giám sát toàn bộ ứng dụng.

---

### 4. Triển khai kỹ thuật

Dự án triển khai qua 4 giai đoạn:
- Nghiên cứu kiến trúc: Thiết kế luồng dữ liệu khớp với danh sách dịch vụ AWS.
- Xây dựng bản Local-first: Hoàn thiện pipeline nội bộ với Docker Compose.
- Chuẩn hóa Cloud-ready: Bắt đầu thay thế các container bằng AWS Services (App Runner, Bedrock, RDS).
- Triển khai & Kiểm thử AWS: Sử dụng CI/CD để build Docker images đẩy lên ECR, cấu hình giám sát CloudWatch và X-Ray.

#### Quyết định kỹ thuật chính
- pgvector thay ChromaDB: Loại bỏ dịch vụ vector DB riêng; lưu vector 1024-dim trực tiếp trong PostgreSQL — đơn giản hóa hạ tầng và cho phép JOIN giữa metadata và vectors.
- App Runner thay Lambda: FastAPI với WebSocket và AI processing chạy lâu cần container liên tục, không phải function invocation.
- Exponential Backoff cho Bedrock: Bedrock giới hạn mặc định 100 requests/phút; implement adaptive retry với botocore.config.
- AI Provider Abstraction: Pattern interface cho phép chuyển đổi giữa Ollama local và AWS Bedrock qua config — không cần thay đổi code pipeline.

---

### 5. Kế hoạch triển khai và mốc thời gian

| Giai đoạn | Thời gian | Milestone |
|---|---|---|
| Nền tảng | Tuần 1–4 | AWS fundamentals: EC2, S3, Lambda, VPC, DynamoDB, API Gateway |
| Sprint 1 | Tuần 5–6 | Docker Compose stack, FastAPI backend, AI pipeline prototype |
| Sprint 2 | Tuần 7–8 | Full AI pipeline, ChromaDB → pgvector, Figma UI design |
| Sprint 3 | Tuần 9–10 | AWS Bedrock integration, Asset Detail Page, WebSocket real-time |
| Sprint 4 | Tuần 11 | AWS Fargate deployment, E2E finalization, clean architecture |

---

### 6. Ước tính chi phí (Môi trường AWS)

Với quy mô kiến trúc production hoàn chỉnh sử dụng hơn 20 dịch vụ AWS như trên (bao gồm Multi-AZ và NAT Gateway chạy 24/7), ước tính chi phí cơ bản cho một môi trường nhỏ/vừa như sau:

| Dịch vụ AWS | Mục đích sử dụng | Ước tính chi phí / Tháng (USD) |
|---|---|---|
| Amazon RDS (PostgreSQL) | Database chính (Multi-AZ, db.t4g.medium, 20GB) | $68.00 |
| AWS NAT Gateway | Cho phép Private Subnet ra Internet (1 NAT, 24/7) | $32.40 |
| Amazon ElastiCache | Cache (Multi-AZ, cache.t4g.micro, 2 nodes) | $32.00 |
| Amazon ECS (Fargate) | Chạy Auto Scaling Task xử lý Video (~1 vCPU, 2GB RAM) | $15.00 |
| Amazon Bedrock & Transcribe | Xử lý AI, Speech-to-Text, Embeddings (Ước lượng cơ bản) | $15.00 |
| AWS App Runner | Web Service / API (1 instance tối thiểu) | $7.00 |
| AWS WAF | Web Application Firewall (1 Web ACL, 1 Rule) | $6.00 |
| Amazon CloudWatch & X-Ray | Logs, Metrics, Alarms, Distributed Tracing | $5.00 |
| Amazon S3 | Lưu trữ Media & Keyframes (Ước tính ~50GB) | $2.00 |
| AWS Secrets Manager | Quản lý Keys, Credentials (5 secrets) | $2.00 |
| Amazon API Gateway | REST API & WSS Push (Theo request) | $1.00 |
| Amazon CloudFront | CDN phân phối nội dung (Data Transfer) | $1.00 |
| Amazon SQS, EventBridge, Step Functions | Điều phối Workflow, Event Bus, Hàng đợi | $1.00 |
| Amazon Route 53 | Quản lý DNS (1 Hosted Zone) | $0.50 |
| Amazon Cognito | Xác thực người dùng (JWT) | $0.00 (Free Tier) |
| AWS Lambda | Lưu Metadata & Embeddings | $0.00 (Free Tier) |
| **Tổng cộng ước tính** | **Toàn bộ hệ thống** | **~$187.90** |

---

### 7. Đánh giá rủi ro

#### Ma trận rủi ro
- Khối lượng media lớn: Ảnh hưởng cao, xác suất trung bình.
- Mô hình AI chạy chậm trên máy local: Ảnh hưởng trung bình, xác suất cao.
- Sai lệch kết quả tìm kiếm: Ảnh hưởng trung bình, xác suất trung bình.
- Chi phí cloud tăng khi scale: Ảnh hưởng cao, xác suất thấp đến trung bình.

#### Chiến lược giảm thiểu
- Dùng pipeline local theo từng bước, cache model và tối ưu batch processing.
- Tách rõ metadata, vector search và object storage để dễ thay thế khi lên AWS.
- Giới hạn ingest theo đợt và theo dõi hiệu năng bằng log/metrics.
- Chỉ đưa các thành phần cần scale lên cloud thay vì chuyển toàn bộ ngay từ đầu.

#### Kế hoạch dự phòng
- Nếu AI inference local quá nặng, có thể giảm kích thước model hoặc chuyển sang Bedrock ở giai đoạn cloud.
- Nếu vector search trong local chậm, có thể chuyển sang OpenSearch Serverless sớm hơn.
- Nếu pipeline ingest lỗi, hệ thống vẫn giữ metadata cơ bản để tránh mất toàn bộ thư viện media.

---

### 8. Kết quả kỳ vọng

- Về mặt kỹ thuật: SMA cho phép người dùng nạp media, tự động tạo chỉ mục ngữ nghĩa, tìm kiếm bằng ngôn ngữ tự nhiên và nhảy trực tiếp tới đúng timestamp trong video.
- Về mặt sản phẩm: Hệ thống phù hợp cho nhóm thiết kế, dựng phim, nghiên cứu nội dung hoặc phòng lab cần quản lý media nội bộ một cách an toàn và hiệu quả.
- Về mặt mở rộng: Kiến trúc local-first giúp phát triển nhanh, còn mapping sang AWS giúp hệ thống có đường nâng cấp rõ ràng lên môi trường production khi cần.