---
title: "Bản đề xuất"
date: 2026-05-17
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Smart Media Analytics
## Hệ thống phân tích và truy vấn video thông minh (Video Semantic Search)

### 1. Tóm tắt điều hành

Smart Media Analytics (SMA) là nền tảng phân tích đa phương tiện theo mô hình local-first, được xây dựng để tự động hóa toàn bộ các bước từ tiếp nhận dữ liệu, nhận dạng cảnh quay, phiên âm nội dung âm thanh đến truy vấn ngữ nghĩa theo thời gian thực. Với SMA, người dùng chỉ cần mô tả nội dung cần tìm bằng câu chữ thông thường như "cảnh hoàng hôn bên bờ biển" để hệ thống trả về đúng đoạn video cùng mốc thời gian tương ứng, thay vì phải xem lại từng phút footage một cách thủ công. Hệ thống vận hành khép kín trong môi trường Docker ở giai đoạn thử nghiệm nhằm kiểm soát chi phí và bảo đảm an toàn dữ liệu, đồng thời được thiết kế sẵn sàng chuyển đổi lên hạ tầng AWS (S3, Bedrock, OpenSearch, RDS) khi có nhu cầu mở rộng quy mô.

Tài nguyên dự án:
- GitHub: https://github.com/ntnhan19/smart_media_analytics_cloudforge


---

### 2. Vấn đề

Bối cảnh thực tế: Kho lưu trữ media ngày càng phình to trong khi cách đặt tên tệp rời rạc, thiếu tính mô tả đã khiến công việc tìm kiếm footage trở thành một bài toán tốn công. Mỗi lần cần truy hồi một cảnh quay cụ thể, nhóm sản xuất phải mở từng tệp, tua đi tua lại qua nhiều đoạn dài trước khi xác định được nội dung cần dùng. Đưa toàn bộ dữ liệu lên Cloud AI ngay từ đầu cũng không phải lựa chọn tối ưu vì vừa đội chi phí, vừa mang rủi ro lộ lọt thông tin nội bộ.

Cách tiếp cận của SMA: Hệ thống xây dựng một pipeline xử lý tự động hoàn toàn ở phạm vi nội bộ, kết hợp React dashboard cho phép quản lý asset trực quan, FastAPI đảm nhận cổng API, ChromaDB dùng để lập chỉ mục vector embedding, PostgreSQL lưu trữ dữ liệu thuộc tính, MinIO quản lý object media, Ollama chạy các mô hình nhận dạng hình ảnh và sinh embedding ngay trên máy cục bộ, cùng faster-whisper để chuyển giọng nói thành văn bản. Khi sẵn sàng lên mây, từng thành phần trong stack này sẽ được hoán đổi sang dịch vụ AWS tương ứng: MinIO → Amazon S3, mô hình Ollama → AWS Bedrock, faster-whisper → Amazon Transcribe, PostgreSQL → Amazon RDS. Hệ sinh thái đám mây còn được bổ sung thêm CloudFront, Cognito, WAF, ECR, CloudWatch, Secrets Manager, X-Ray, SQS, EventBridge, Step Functions, API Gateway, App Runner, ECS Fargate, Lambda, ElastiCache và VPC để hoàn thiện vòng đời vận hành.

Hiệu quả thực tiễn: Thay vì lướt xem từng tệp, người dùng nhập một truy vấn bằng ngôn ngữ hằng ngày như "cảnh hoàng hôn trên biển có tiếng sóng" và nhận lại ngay danh sách các đoạn video khớp cùng timestamp chính xác. Điều này giúp các nhóm nội dung tiết kiệm đáng kể thời gian sàng lọc, đọc lời thoại và biên tập footage.

---

### 3. Kiến trúc giải pháp

Kiến trúc của SMA áp dụng mô hình lai hybrid local-to-cloud nhằm duy trì tính linh hoạt trong quá trình lập trình nội bộ đồng thời giảm thiểu rủi ro khi chuyển đổi lên môi trường đám mây quy mô lớn. Ở mức độ cục bộ, toàn bộ chuỗi nạp và phân tích dữ liệu được đóng gói trong Docker Compose: thực hiện tiếp nhận video, phân tách cảnh, xử lý âm thanh, sinh mô tả nội dung tự động thông qua AI và lưu trữ dữ liệu cục bộ.

![Kiến trúc hệ thống](/images/2-Proposal/platform_architecture.png)

Khi chuyển dịch lên AWS, hệ thống dễ dàng mở rộng quy mô xử lý mà không cần cấu trúc lại các logic cốt lõi. Amazon S3 tiếp quản lưu trữ media, AWS Bedrock và Amazon Transcribe xử lý các tác vụ học máy, RDS PostgreSQL quản lý metadata thuộc tính. Việc phân phối giao diện frontend được tối ưu hóa qua CloudFront và S3, cùng với sự hỗ trợ của Cognito, WAF, ECR, Lambda, ECS Fargate, App Runner, Step Functions, SQS, EventBridge, CloudWatch, X-Ray, ElastiCache và VPC để đảm bảo an ninh, điều phối luồng công việc và giám sát hệ thống.

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

Quy trình phát triển dự án được tổ chức chặt chẽ qua 4 giai đoạn nối tiếp:
- Thiết lập sơ đồ: Mô hình hóa các luồng truyền nhận thông tin tương thích với đặc tả hệ thống AWS.
- Phát triển bản local: Định hình và kiểm thử pipeline nội bộ thông qua Docker Compose.
- Chuyển đổi cloud-ready: Thay thế dần các dịch vụ Docker local bằng các dịch vụ đám mây AWS tương ứng (như App Runner, Bedrock, RDS).
- Kiểm thử tích hợp AWS: Thiết lập quy trình CI/CD đóng gói Docker image đẩy lên ECR, cấu hình hệ thống ghi log và giám sát phân tán với CloudWatch và X-Ray.

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

Hạ tầng production được thiết kế tối ưu trên môi trường đám mây AWS với các dịch vụ bổ trợ chạy liên tục 24/7. Dưới đây là ước tính chi phí vận hành cơ bản đối với một hệ thống quy mô nhỏ và vừa:

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

- Về mặt kỹ thuật: Hệ thống SMA mang đến khả năng nạp liệu và lập chỉ mục nội dung video tự động, cho phép định vị cảnh quay dựa trên ngôn ngữ tự nhiên.
- Về mặt sản phẩm: Giải pháp hoàn thiện tối ưu cho các nhóm sản xuất media, dựng phim hoặc khối nghiên cứu cần lưu trữ và truy xuất tư liệu một cách tin cậy và bảo mật.
- Về mặt mở rộng: Kiến trúc local-first giúp tối ưu tiến độ lập trình thử nghiệm, trong khi thiết kế AWS sẵn sàng hỗ trợ việc dịch chuyển hạ tầng lên production khi lưu lượng tăng cao.