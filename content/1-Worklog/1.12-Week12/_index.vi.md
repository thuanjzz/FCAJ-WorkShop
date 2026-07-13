---
title: "Worklog Tuần 12"
date: 2026-07-06
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Mục tiêu tuần 12 (06/07 – 12/07/2026):

* Triển khai toàn bộ hạ tầng AWS cho hệ thống Smart Media Analytics lên môi trường production.
* Thiết lập pipeline CI/CD tự động và hệ thống giám sát.
* Soạn thảo tài liệu Workshop hướng dẫn triển khai kiến trúc dự án.

### Các công việc triển khai trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | Khởi tạo hạ tầng mạng (VPC, Subnets, NAT Gateway, Security Groups) và cơ sở dữ liệu (RDS PostgreSQL với pgvector, ElastiCache Redis) trên AWS | 06/07/2026 | 06/07/2026 | AWS VPC, RDS docs |
| 3   | Xây dựng luồng xử lý AI tự động: cấu hình SQS, EventBridge, AWS Step Functions và tích hợp Amazon Bedrock (Nova Lite, Titan Embeddings) | 07/07/2026 | 07/07/2026 | AWS Step Functions docs |
| 4   | Thiết lập pipeline CI/CD tự động deploy Backend API và AI Worker lên Amazon ECS Fargate thông qua GitHub Actions và Amazon ECR | 08/07/2026 | 08/07/2026 | GitHub Actions, ECR docs |
| 5   | Cấu hình Application Load Balancer (ALB) và tích hợp thành công luồng WebSocket real-time cho ECS Backend | 09/07/2026 | 09/07/2026 | AWS ALB docs |
| 6   | Triển khai Frontend lên AWS Amplify kết hợp Amazon Cognito xác thực người dùng và Route 53 quản lý DNS | 10/07/2026 | 10/07/2026 | AWS Amplify docs |
| 7   | Thiết lập hệ thống giám sát toàn diện: Amazon CloudWatch (Logs, Metrics, Alarms) và AWS X-Ray distributed tracing | 11/07/2026 | 11/07/2026 | CloudWatch, X-Ray docs |
| CN  | Soạn thảo và hoàn thiện tài liệu Workshop hướng dẫn triển khai toàn bộ kiến trúc hệ thống Smart Media Analytics trên AWS | 12/07/2026 | 12/07/2026 | |

### Kết quả đạt được tuần 12:

* Hạ tầng mạng AWS hoàn chỉnh: VPC với public/private subnets, NAT Gateway, Security Groups theo nguyên tắc least privilege.
* Cơ sở dữ liệu production: RDS PostgreSQL Multi-AZ với extension pgvector; ElastiCache Redis cho cache và Pub/Sub.
* Luồng xử lý AI tự động: Video tải lên S3 → SQS → EventBridge → Step Functions → ECS AI Worker (Bedrock + Transcribe) hoạt động end-to-end.
* CI/CD tự động: GitHub Actions build và push Docker image lên ECR; ECS service tự động cập nhật khi có image mới.
* Load Balancer và WebSocket: ALB phân phối traffic đến ECS Backend; WebSocket kết nối ổn định qua ALB listener rules.
* Frontend production: Amplify hosting với CDN toàn cầu; Cognito xác thực JWT; Route 53 quản lý tên miền.
* Observability: CloudWatch dashboard theo dõi CPU, memory, request count và error rate; X-Ray tracing toàn bộ request pipeline.
* Tài liệu Workshop hoàn chỉnh: Hướng dẫn step-by-step triển khai từng thành phần kiến trúc, bao gồm sơ đồ, lệnh CLI và ảnh minh họa.
