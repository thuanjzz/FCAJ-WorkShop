---
title: "Event 1"
date: 2026-04-17
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# First Cloud AI Journey — Cohort Kickoff & AWS Orientation

### 1. Mục đích của sự kiện

Sự kiện **Cohort Kickoff & AWS Orientation** đánh dấu sự khởi đầu chính thức của chương trình thực tập First Cloud AI Journey (FCAJ). Sự kiện nhằm mục đích:
- Kết nối các thực tập sinh với mentor doanh nghiệp và các thành viên trong nhóm.
- Thống nhất các kỳ vọng về chất lượng code, quy tắc cộng tác và quy trình Agile (Scrum) 2 tuần/sprint.
- Trình bày chi tiết mục tiêu kinh doanh và phạm vi kỹ thuật của dự án: **Smart Media Analytics CloudForge** (Video Semantic Search).
- Hướng dẫn thực tập sinh cấu hình IAM credentials, cài đặt AWS CLI và làm quen với AWS Management Console.

---

### 2. Diễn giả & Mentor

- **Nguyễn Gia Hưng** — Senior Cloud Solutions Architect & Mentor doanh nghiệp (hunggia@amazon.com).
- **FCAJ Admin Team** — Điều phối chương trình và HLV Agile.

---

### 3. Nội dung nổi bật & Hoạt động chính

#### Thiết lập quy trình Agile & Scrum
- Thiết lập chu kỳ 2 tuần cho mỗi sprint.
- Giới thiệu GitHub Issues Board để theo dõi tiến độ công việc, quy trình PR review và log issue (ví dụ: quy định đặt tên nhánh Git).
- Làm rõ định nghĩa hoàn thành (Definition of Done - DoD): code sạch, pass linter local và build Docker thành công.

#### Định hướng phạm vi dự án
- **Nhiệm vụ:** Xây dựng công cụ tìm kiếm ngữ nghĩa video bằng AI.
- **Phân công vai trò:** Phạm Ninh Thuận chịu trách nhiệm chính về **module AI Pipeline** (Scene detection, ASR transcription, Bedrock image captioning và lưu trữ vector).
- **Bản thảo kiến trúc:** Thảo luận về sơ đồ mapping giữa local (Docker) ↔ cloud (AWS App Runner, RDS pgvector, Bedrock, S3).

#### Thực hành cấu hình AWS
- Hướng dẫn tạo IAM user tuân thủ nguyên tắc quyền hạn tối thiểu (least privilege).
- Chuẩn hóa cấu hình AWS CLI: cài đặt Access Key, Secret Key, default region (`us-east-1`) và cấu hình MFA cho tài khoản root.

---

### 4. Bài học rút ra

- **Tư duy làm chủ (Ownership):** Là người sở hữu AI pipeline, tôi nhận ra cần thiết lập API contract sớm để không làm nghẽn tiến độ của team frontend và database.
- **Nguyên tắc quyền hạn tối thiểu:** Cấu hình IAM policy với quyền hạn hẹp nhất có thể (tránh dùng wildcard `*`) để đảm bảo an toàn cho tài nguyên đám mây.
- **Kỷ luật Agile:** Học cách ước lượng công việc (task estimation) và chia nhỏ các bài toán kiến trúc phức tạp thành các task nhỏ hơn trên GitHub Issues.

---

### 5. Ứng dụng vào công việc dự án

Ngay sau sự kiện, tôi đã cấu hình thành công AWS CLI local, tạo môi trường Docker Compose ban đầu và viết bản thảo OpenAPI Data Contract đầu tiên, tạo nền móng vững chắc cho Sprint 1.
