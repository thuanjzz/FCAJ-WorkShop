---
title: "Event 2"
date: 2026-06-20
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# AWS Community Day Vietnam 2026

### 1. Mục đích của sự kiện

**AWS Community Day Vietnam 2026** là hội nghị công nghệ quy mô lớn do cộng đồng AWS Việt Nam tổ chức dành cho các cloud developer, kỹ sư hệ thống và sinh viên ngành IT. Sự kiện nhằm mục đích:
- Chia sẻ kinh nghiệm thực tế khi vận hành hệ thống đám mây (cloud-native) trên production.
- Trình bày chuyên sâu về các dịch vụ hàng đầu như **AWS Bedrock (Generative AI)**, **ECS Fargate** và **Serverless scaling**.
- Tạo cơ hội kết nối, giao lưu giữa sinh viên và các chuyên gia giải pháp AWS (AWS Solutions Architects).

---

### 2. Các session nổi bật đã tham dự

#### Session 1: Xây dựng ứng dụng GenAI quy mô lớn với AWS Bedrock
- Phân tích giới hạn rate limit, cách đánh giá mô hình và tối ưu hóa throughput của Bedrock.
- Thảo luận các chiến lược giảm thiểu lỗi `ThrottlingException` (HTTP 429) bằng adaptive throttling và backoff phía client — kiến thức trực tiếp giải quyết vấn đề trong dự án của tôi (Issue #72).

#### Session 2: Vận hành Container thực tế trên ECS Fargate
- Tập trung vào tối ưu hóa kích thước Docker image, bảo mật cho Task Definition và tận dụng VPC endpoints để tải tài nguyên riêng tư.
- Chia sẻ các mô hình điều phối short-running ECS Fargate tasks bằng AWS Step Functions.

#### Session 3: Vector Database trên AWS RDS với pgvector
- Phân tích chuyên sâu về cách dùng PostgreSQL + pgvector cho tìm kiếm tương đồng (semantic search).
- So sánh hiệu năng giữa IVFFlat và HNSW indexes, chứng minh HNSW giúp giảm độ trễ truy vấn đáng kể ở quy mô lớn.

---

### 3. Bài học và Giá trị kỹ thuật đạt được

- **Tự động phục hồi lỗi AI:** Nhận thấy việc áp dụng exponential backoff là bắt buộc khi gọi hàng loạt API Bedrock. Session này củng cố quyết định dùng thư viện `tenacity` của tôi để xử lý lỗi 429.
- **Tối ưu hóa HNSW Index:** Hiểu rõ tại sao HNSW vượt trội hơn scan tuần tự đối với tìm kiếm embedding. Tôi đã triển khai index này ở Tuần 11 để tối ưu hóa pgvector.
- **Tối ưu hóa Private Link:** Biết cách dùng Gateway và Interface endpoints trong VPC để tránh phí truyền tải dữ liệu khi các ECS task tải dữ liệu từ Amazon S3.

---

### 4. Ứng dụng vào dự án thực tập

Sự kiện diễn ra vào Tuần 9 của kỳ thực tập. Kiến thức từ các session Bedrock và pgvector đã giúp tôi di chuyển thành công hệ thống từ ChromaDB sang **RDS pgvector**, đồng thời triển khai cơ chế **Adaptive Retry với Exponential Backoff** cho module Bedrock Nova Lite captioning, cải thiện đáng kể độ tin cậy của pipeline.
