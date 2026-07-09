---
title: "Các bài blog đã đăng"
date: 2026-07-05
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Trong thời gian thực tập tại FCAJ, tôi viết và đăng các bài blog kỹ thuật lên cộng đồng [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj) để chia sẻ kiến thức và ghi lại bài học.

### [Blog 1 — Xây dựng ứng dụng tìm kiếm đầu tiên với Amazon OpenSearch Service](3.1-Blog1/)

Bài viết giới thiệu về Amazon OpenSearch Service - dịch vụ tìm kiếm và phân tích được quản lý hoàn toàn bởi AWS. Bài viết giải thích các khái niệm cốt lõi (Document, Index, Cluster, Node, Shard, Replica, Inverted Index) và cung cấp kiến trúc ứng dụng mẫu bảo mật nhiều lớp kết hợp AWS App Runner, Cognito, API Gateway, Lambda bên trong VPC riêng tư.

### [Blog 2 — Tự động số hóa hồ sơ bệnh án với Amazon Bedrock Data Automation và AWS HealthLake](3.2-Blog2/)

Bài viết chia sẻ một kiến trúc serverless kết hợp AI giúp tự động số hóa hồ sơ bệnh án không cấu trúc (file PDF hoặc hình ảnh scan) sử dụng Amazon Bedrock Data Automation và AWS HealthLake. Bài viết trình bày chi tiết luồng xử lý dạng event-driven, lợi ích của việc chuẩn hóa dữ liệu theo chuẩn y tế toàn cầu FHIR R4, các trường hợp sử dụng lâm sàng thực tế và các lưu ý bảo mật cần thiết khi triển khai.

### [Blog 3 — AI và Robot đang thay đổi nông nghiệp bền vững như thế nào với Amazon SageMaker AI?](3.3-Blog3/)

Bài viết khám phá cách Aigen — công ty phát triển robot nông nghiệp tự hành — hiện đại hóa toàn bộ pipeline AI bằng Amazon SageMaker AI. Bài viết trình bày kiến trúc cloud-native với vòng lặp học liên tục, kỹ thuật Active Learning giúp giảm chi phí gán nhãn, các use case thực tế như nhận diện cỏ dại và cải thiện mô hình liên tục, cùng các kết quả cụ thể — bao gồm tăng 20 lần năng suất gán nhãn và giảm 22,5 lần chi phí gán nhãn mỗi hình ảnh.

### [Blog 4 — Deep Dive vào AWS Bedrock & Vector Embedding – Cái gì biến văn bản thô thành "Tọa độ" cho AI?](3.4-Blog4/)

Bài viết giải mã quá trình Embedding — nền tảng của mọi hệ thống RAG (Retrieval-Augmented Generation). Viết từ góc nhìn của một Backend Developer, bài viết dẫn dắt toàn bộ pipeline Chunking → Bedrock Embedding → Vector Search, giải thích lý do chọn Amazon Bedrock thay vì tự host model, và chia sẻ các bài học thực tế về rate limiting, chiến lược chunking và quản lý chi phí khi xây dựng tính năng tìm kiếm AI trên AWS.