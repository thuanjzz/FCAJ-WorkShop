---
title: "Blog 1"
date: 2026-07-05
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# VPC Endpoints: Truy cập Amazon S3 Riêng tư và Bảo mật

### 1. Giới thiệu

Trong kiến trúc đám mây hiện đại, bảo mật và tối ưu chi phí là hai yếu tố hàng đầu. Khi các tài nguyên tính toán bên trong mạng riêng (VPC) cần giao tiếp với các dịch vụ lưu trữ như Amazon S3, việc định tuyến lưu lượng qua internet công cộng là một điểm yếu bảo mật và làm phát sinh chi phí truyền tải dữ liệu NAT Gateway không đáng có.

AWS PrivateLink giải quyết vấn đề này bằng cách cung cấp kết nối riêng tư thông qua VPC Endpoints. Bài viết này sẽ hướng dẫn cách cấu hình và kiểm tra Gateway và Interface VPC Endpoints để truy cập S3 bảo mật.

---

### 2. So sánh Gateway Endpoints vs. Interface Endpoints

| Đặc điểm | Gateway Endpoint | Interface Endpoint |
|---|---|---|
| **Công nghệ** | Định tuyến qua Prefix List trong Route Table | Cấp phát Elastic Network Interface (ENI) với IP riêng |
| **Cách truy cập** | Cập nhật bảng định tuyến | Phân giải qua DNS |
| **Phạm vi** | Chỉ trong VPC | Trong VPC & On-premises (qua VPN/Direct Connect) |
| **Chi phí** | Miễn phí | Tính phí theo giờ + Phí xử lý dữ liệu |

---

### 3. Cấu hình Gateway VPC Endpoint cho S3

Đối với các EC2 instance chạy trong subnet riêng tư (private subnet) trong VPC Cloud, **Gateway Endpoint** là lựa chọn tối ưu nhất về hiệu năng và chi phí.

#### Bước 1: Tạo Endpoint
1. Mở **Amazon VPC console**.
2. Chọn **Endpoints** -> **Create endpoint**.
3. Service category: Chọn **AWS services**.
4. Service name: Tìm kiếm từ khóa `s3` và chọn dịch vụ có type là **Gateway** (ví dụ: `com.amazonaws.us-east-1.s3`).
5. VPC: Chọn VPC Cloud của bạn.

#### Bước 2: Cấu hình bảng định tuyến (Route Table)
1. Ở phần **Route tables**, chọn bảng định tuyến liên kết với các private subnet của bạn.
2. Hệ thống sẽ tự động thêm một dòng định tuyến trỏ đến S3 prefix list (ví dụ: `pl-63a5400a`) với target là Endpoint ID (`vpce-xxxxxx`).

#### Bước 3: Xác minh kết nối
SSH vào EC2 instance trong private subnet và chạy lệnh:
```bash
aws s3 ls --region us-east-1
```
Nếu cấu hình đúng, danh sách S3 buckets sẽ hiển thị ngay lập tức mà không cần đi qua Internet Gateway.

---

### 4. Cấu hình Interface VPC Endpoint cho kết nối On-Premises (Hybrid)

Nếu bạn cần truy cập Amazon S3 từ mạng on-premises (nội bộ doanh nghiệp) thông qua VPN hoặc AWS Direct Connect, bạn không thể sử dụng Gateway Endpoint. Thay vào đó, bạn phải tạo một **Interface Endpoint**.

#### Bước 1: Tạo Endpoint
1. Tại VPC Console, chọn **Create endpoint**.
2. Chọn **AWS services** -> Tìm `s3` -> Chọn loại **Interface**.
3. Chọn VPC và chọn các subnet cụ thể để cấp phát card mạng ảo ENI.
4. Bật tính năng **Private DNS** để các truy vấn tự động phân giải về IP riêng tư của Endpoint.

#### Bước 2: Kiểm thử kết nối từ On-Premises
Sử dụng DNS của Interface Endpoint để truy vấn S3 từ máy trạm on-premises:
```bash
aws s3 ls --endpoint-url https://bucket.vpce-xxxxxx.s3.us-east-1.vpce.amazonaws.com
```

---

### 5. Kết luận

Triển khai VPC Endpoints là một thực hành bảo mật cốt lõi trên AWS. Định tuyến lưu lượng S3 nội bộ giúp bảo vệ dữ liệu khỏi internet công cộng và giảm thiểu đáng kể chi phí băng thông NAT Gateway.