# Smart Media Analytics — Hugo Workshop Website & Internship Report

[![Hugo Version](https://img.shields.io/badge/Hugo-v0.120+-blue.svg?style=flat-square&logo=hugo)](https://gohugo.io/)
[![Deployment](https://img.shields.io/badge/Deployed%20to-GitHub%20Pages-green.svg?style=flat-square&logo=github)](https://thuanjzz.github.io/FCAJ-WorkShop/)
[![AWS Architecture](https://img.shields.io/badge/AWS-Serverless%20%26%20AI-orange.svg?style=flat-square&logo=amazon-aws)](https://aws.amazon.com/)

Chào mừng bạn đến với kho lưu trữ mã nguồn của **Workshop Website & Báo cáo thực tập** cá nhân tại **First Cloud AI Journey (FCAJ)** (diễn ra từ **17/04/2026** đến **17/07/2026**). 

Website này tài liệu hóa toàn bộ quá trình thiết kế, triển khai dự án **Smart Media Analytics** trên hạ tầng đám mây AWS kết hợp Generative AI.

* **Website thực tế:** [https://thuanjzz.github.io/FCAJ-WorkShop/](https://thuanjzz.github.io/FCAJ-WorkShop/)

---

## 🏗️ Tổng quan Dự án: Smart Media Analytics (Team CloudForge)

**Smart Media Analytics** là giải pháp phân tích video toàn diện sử dụng kiến trúc **Serverless**, **Container** và các mô hình **Generative AI** nâng cao trên nền tảng AWS. Hệ thống cho phép tự động điều phối, xử lý video nặng, trích xuất văn bản (speech-to-text), tạo mô tả ngữ cảnh hình ảnh và hỗ trợ **Tìm kiếm Ngữ nghĩa (Semantic Search)** qua dữ liệu Vector.

### Các thành phần kiến trúc chính:
1. **Frontend & Authentication:** Ứng dụng Web (React) triển khai trên **AWS Amplify**, phân giải tên miền qua **Route 53**, xác thực bảo mật bằng **Cognito** và kết nối Backend qua **API Gateway** & **ALB**.
2. **Ingestion & Orchestration:** Video tải lên **S3** kích hoạt sự kiện qua **SQS**, **EventBridge** tới **AWS Step Functions** để điều phối toàn bộ pipeline xử lý video tự động.
3. **AI Pipeline & Real-time Processing:** **ECS AI Worker (AWS Fargate)** kết nối với **Amazon Transcribe** và **Amazon Bedrock (Nova Lite & Titan Embeddings)** để xử lý đa phương thức và tạo vector nhúng. Tiến trình xử lý thời gian thực được đồng bộ qua **ElastiCache for Redis** và **WebSocket**.
4. **Data Persistence & Search:** Lưu trữ siêu dữ liệu và thực hiện tìm kiếm Vector (Vector Search) bằng extension `pgvector` trên **Amazon RDS PostgreSQL** (Multi-AZ).
5. **CI/CD & Observability:** Xây dựng pipeline tự động hóa bằng **GitHub Actions** (đẩy Docker Image lên **ECR**) và giám sát hệ thống thông qua **Amazon CloudWatch** kết hợp **AWS X-Ray** sidecar daemon.

---

## 📁 Cấu trúc Thư mục Hugo

Dự án này sử dụng công cụ tĩnh **Hugo** với chủ đề `hugo-theme-learn` hỗ trợ đa ngôn ngữ (Tiếng Anh và Tiếng Việt):

```bash
FCAJ-WorkShop/
├── archetypes/          # Khuôn mẫu định dạng bài viết mới
├── config.toml          # Tệp cấu hình chính của Hugo (metadata, đa ngôn ngữ, menu)
├── content/             # Nội dung các trang Markdown (.md & .vi.md)
│   ├── 1-Worklog/       # Nhật ký thực tập theo tuần
│   ├── 2-Proposal/      # Đề xuất dự án & Phân tích yêu cầu
│   ├── 3-BlogsPosted/   # Các bài viết blog chia sẻ kiến thức công nghệ
│   ├── 4-EventParticipated/ # Các sự kiện, workshop của cộng đồng đã tham gia
│   ├── 5-Workshop/      # Tài liệu thực hành các chương xây dựng hệ thống (AWS)
│   ├── 6-Self-evaluation/ # Tự đánh giá kết quả thực tập cá nhân
│   └── 7-Feedback/      # Chia sẻ và đóng góp ý kiến cuối khóa
├── static/              # Các tài nguyên tĩnh (Hình ảnh, Sơ đồ kiến trúc, CSS, JS)
│   └── images/          # Ảnh minh họa phân loại theo từng phần tương ứng
└── themes/              # Thư mục theme của Hugo (hugo-theme-learn là git submodule)
```

---

## 🚀 Hướng dẫn Chạy Website Dưới Local

Để xem và chỉnh sửa giao diện website dưới local của bạn, hãy thực hiện các bước sau:

### 1. Sao chép kho lưu trữ
Do theme được quản lý dưới dạng Git Submodule, hãy sử dụng tùy chọn `--recursive` khi clone:
```bash
git clone --recursive https://github.com/thuanjzz/FCAJ-WorkShop.git
cd FCAJ-WorkShop
```
*Nếu bạn đã clone bình thường, hãy chạy lệnh sau để cập nhật theme:*
```bash
git submodule update --init --recursive
```

### 2. Cài đặt Hugo
* Đảm bảo máy tính của bạn đã cài đặt phiên bản **Hugo Extended** (khuyên dùng v0.120.0 trở lên).
* Bạn có thể cài đặt nhanh qua chocolatey (Windows):
  ```powershell
  choco install hugo-extended
  ```

### 3. Chạy môi trường thử nghiệm
Chạy lệnh phát triển cục bộ của Hugo:
```bash
hugo server
```
Website sẽ được dựng và chạy tại địa chỉ: **[http://localhost:1313/FCAJ-WorkShop/](http://localhost:1313/FCAJ-WorkShop/)**

---

## 🌐 Triển khai & Tự động hóa

Mỗi khi có commit mới được đẩy (push) lên nhánh `main`, một quy trình tự động của **GitHub Actions** sẽ được kích hoạt để kiểm tra, biên dịch mã nguồn Hugo thành các tệp HTML tĩnh và tự động deploy lên **GitHub Pages** giúp cập nhật website tức thời.

---
*Chúc bạn có những trải nghiệm học tập và thực hành hiệu quả với dự án Smart Media Analytics!*
