---
title: "Các bài blog đã đăng"
date: 2026-07-05
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Trong thời gian thực tập tại FCAJ, tôi viết và đăng các bài blog kỹ thuật lên cộng đồng [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj) để chia sẻ kiến thức và ghi lại bài học.

### [Blog 1 — VPC Endpoints: Truy cập Amazon S3 Riêng tư và Bảo mật](3.1-Blog1/)

Bài blog giải thích hai loại VPC endpoints cho S3 — **Gateway Endpoint** (cho EC2 trong VPC) và **Interface Endpoint** (cho on-premises qua VPN/Direct Connect). Bao gồm use cases, các bước cấu hình và cách xác minh traffic đi trong mạng riêng AWS mà không qua internet công cộng. Chủ đề này được ứng dụng trực tiếp trong workshop FCAJ về **Secure Hybrid Access to S3**.

### [Blog 2 — AWS Bedrock Throttling: Chiến lược Exponential Backoff cho Hệ thống AI Production](3.2-Blog2/)

Bài blog ghi lại vấn đề **ThrottlingException** gặp phải khi tích hợp AWS Bedrock (Amazon Nova Lite) vào AI pipeline trong kỳ thực tập. Giải thích nguyên nhân (giới hạn mặc định 100 requests/phút), giải pháp adaptive retry dùng `botocore.config`, và cách kiểm thử với `botocore.stub.Stubber`. Bài viết tham chiếu GitHub Issue #72 từ dự án `smart_media_analytics_cloudforge`. Hữu ích cho các nhóm xây dựng hệ thống xử lý AI theo batch trên Bedrock.

### [Blog 3 — Từ ChromaDB sang pgvector: Đơn giản hóa Stack AI](3.3-Blog3/)

Bài blog chia sẻ quyết định kiến trúc chuyển từ **ChromaDB** (vector database riêng biệt) sang **pgvector** (extension PostgreSQL). Bao gồm lý do (loại bỏ service riêng, cho phép JOIN giữa metadata và vectors), các bước migration, so sánh hiệu năng truy vấn và cấu hình HNSW index cho cosine similarity search ở quy mô production. Hướng dẫn thực tế cho các nhóm xây dựng hệ thống semantic search trên AWS RDS.