---
title: "Worklog Tuần 2"
date: 2026-04-27
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2 (27/04 – 03/05/2026):

* Khám phá và thực hành các dịch vụ AWS cốt lõi: S3, Lambda, VPC.
* Hiểu kiến trúc mạng AWS và các mẫu kiến trúc cloud phổ biến.
* Hoàn thành labs thực hành cho từng dịch vụ.

### Các công việc triển khai trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | Tìm hiểu Amazon S3: tạo bucket, lưu trữ object, versioning, lifecycle policies, presigned URLs, static website | 27/04/2026 | 27/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3   | Thực hành S3: Tạo bucket, upload/download object, bật versioning, cấu hình lifecycle rules, host static website | 28/04/2026 | 28/04/2026 | https://docs.aws.amazon.com/s3/ |
| 4   | Tìm hiểu AWS Lambda: kiến trúc event-driven, trigger (S3, API Gateway, SQS), execution environment, cold start, IAM roles | 29/04/2026 | 29/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5   | Thực hành Lambda: Tạo Lambda function Python được trigger bởi S3 event; test với sample events | 30/04/2026 | 30/04/2026 | https://docs.aws.amazon.com/lambda/ |
| 6   | Tìm hiểu VPC: subnets (public/private), route tables, Internet Gateway, NAT Gateway, Security Groups vs NACLs | 01/05/2026 | 02/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| CN  | Thực hành VPC: Tạo VPC tùy chỉnh với public/private subnets; triển khai EC2 trong mỗi subnet; kiểm tra kết nối | 03/05/2026 | 03/05/2026 | https://docs.aws.amazon.com/vpc/ |

### Kết quả đạt được tuần 2:

* Hiểu được kiến thức Amazon S3: bucket policies, versioning, lifecycle rules, static website hosting và presigned URLs.
* Tạo Lambda function Python phản ứng với S3 upload event và ghi log metadata lên CloudWatch.
* Hiểu kiến trúc VPC: CIDR blocks, public vs private subnets, route tables, Internet/NAT Gateway và Security Groups.
* Xây dựng thành công VPC tùy chỉnh với 2 AZ, triển khai EC2 trong cả public lẫn private subnet và xác minh routing.
* Hiểu được cách mà các dịch vụ này kết nối với nhau, tạo nền tảng cho kiến trúc cloud-native.
