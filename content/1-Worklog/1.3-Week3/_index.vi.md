---
title: "Worklog Tuần 3"
date: 2026-05-04
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3 (04/05 – 10/05/2026):

* Hiểu DynamoDB và các khái niệm cơ sở dữ liệu NoSQL.
* Thiết kế bảng DynamoDB với partition key và sort key phù hợp.
* Thực hành CRUD operations dùng AWS SDK (Python boto3).

### Các công việc triển khai trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | Tìm hiểu NoSQL vs. relational DB; các khái niệm DynamoDB: tables, items, attributes, primary keys | 04/05/2026 | 04/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3   | Data modeling DynamoDB: partition key, sort key, GSI (Global Secondary Index), LSI (Local Secondary Index) | 05/05/2026 | 05/05/2026 | https://docs.aws.amazon.com/dynamodb/ |
| 4   | Query vs. Scan: hiệu năng, filter expressions, ProjectionExpression | 06/05/2026 | 06/05/2026 | https://docs.aws.amazon.com/dynamodb/ |
| 5   | Thực hành: Tạo bảng DynamoDB; CRUD với boto3 (Python); Query với GSI | 07/05/2026 | 07/05/2026 | |
| 6   | DynamoDB Streams, TTL, DAX; backup và restore | 08/05/2026 | 08/05/2026 | https://docs.aws.amazon.com/dynamodb/ |
| 7   | Ôn lại kiến thức DynamoDB; so sánh với lựa chọn PostgreSQL + pgvector của dự án | 09/05/2026 | 09/05/2026 | |

### Kết quả đạt được tuần 3:

* Hiểu sự khác biệt cơ bản giữa SQL (relational) và NoSQL và khi nào dùng cái nào.
* Thiết kế schema DynamoDB với partition key + sort key cho access patterns hiệu quả.
* Tạo Global Secondary Indexes (GSIs) để truy vấn linh hoạt không cần full table scan.
* Viết Python script dùng boto3 cho đầy đủ CRUD: PutItem, GetItem, UpdateItem, DeleteItem, Query, Scan.
* Hiểu DynamoDB Streams cho change data capture và TTL cho tự động xóa item.
* Phân tích trade-offs: DynamoDB (schemaless, auto-scale) vs. PostgreSQL (relational, complex queries, pgvector) — xác nhận PostgreSQL + pgvector là lựa chọn đúng cho semantic search của dự án.
