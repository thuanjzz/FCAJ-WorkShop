---
title: "Worklog Tuần 4"
date: 2026-05-11
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4 (11/05 – 17/05/2026):

* Hiểu kiến trúc serverless với API Gateway + Lambda.
* Xây dựng và triển khai REST API serverless cơ bản.
* Test API bằng Postman và AWS Console.

### Các công việc triển khai trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | Tìm hiểu kiến trúc serverless: lợi ích, hạn chế, use cases; API Gateway (REST vs HTTP API) | 11/05/2026 | 11/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3   | API Gateway: resources, methods, stages, deployment; tích hợp Lambda; authorization (IAM, Cognito, Lambda Authorizer) | 12/05/2026 | 12/05/2026 | https://docs.aws.amazon.com/apigateway/ |
| 4   | Lambda nâng cao: environment variables, layers, concurrency, error handling, dead-letter queues | 13/05/2026 | 13/05/2026 | https://docs.aws.amazon.com/lambda/ |
| 5   | Thực hành: Xây dựng CRUD REST API với API Gateway + Lambda + DynamoDB (viết code Lambda và thiết lập DynamoDB tables) | 14/05/2026 | 14/05/2026 | https://docs.aws.amazon.com/apigateway/ |
| 6   | Thực hành: Cấu hình API Gateway integration, thiết lập stage và deploy REST API hoàn chỉnh | 15/05/2026 | 15/05/2026 | https://docs.aws.amazon.com/apigateway/ |
| 7   | Test API bằng Postman: GET, POST, PUT, DELETE; kiểm tra CloudWatch logs để debug | 16/05/2026 | 16/05/2026 | |
| CN  | So sánh serverless (API Gateway + Lambda) vs. containerized (FastAPI + Docker) — xác nhận lựa chọn App Runner của dự án | 17/05/2026 | 17/05/2026 | |

### Kết quả đạt được tuần 4:

* Hiểu mô hình serverless và khi nào phù hợp hơn containerized deployment.
* Xây dựng CRUD REST API hoàn chỉnh với API Gateway + Lambda + DynamoDB bằng Python.
* Deploy API lên stage "dev" và lấy được public invoke URL.
* Test thành công tất cả CRUD endpoints (GET, POST, PUT, DELETE) bằng Postman; kiểm tra request/response payload.
* Debug lỗi Lambda qua CloudWatch Logs: nhận diện cold start latency (~800ms) và tối ưu bằng cách tăng memory.
* Đánh giá trade-offs: với FastAPI backend hỗ trợ WebSocket và AI processing dài, AWS App Runner (containerized) phù hợp hơn Lambda.
