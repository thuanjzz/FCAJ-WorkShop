---
title: "Tự đánh giá"
date: 2026-07-05
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

Trong thời gian thực tập tại First Cloud AI Journey (FCAJ) từ **17/04/2026** đến **17/07/2026**, tôi có cơ hội ứng dụng kiến thức đại học vào môi trường kỹ thuật AI thực tế. Tôi là người chịu trách nhiệm chính về module AI Pipeline của dự án `smart_media_analytics_cloudforge` - bao gồm scene detection, speech transcription, vision captioning, vector embedding, Ingest API và Search API. Ngoài ra còn đóng góp vào frontend (Asset Detail Page, Upload flow với WebSocket), lớp tích hợp AWS Bedrock và thiết kế kiến trúc AWS cloud tổng thể.

Kỳ thực tập này đã giúp mở rộng đáng kể hiểu biết của bản thân về xây dựng hệ thống AI production trên hạ tầng cloud. Dưới đây là đánh giá trung thực của bản thân:

| STT | Tiêu chí | Mô tả | Tốt | Khá | Trung bình |
| --- | -------- | ----- | --- | --- | ---------- |
| 1 | **Kiến thức & kỹ năng chuyên môn** | Ứng dụng AI/ML, FastAPI, AWS Bedrock, pgvector, Docker, React trong dự án thực tế | ☐ | ✅ | ☐ |
| 2 | **Khả năng học hỏi** | Nhanh chóng tiếp thu công nghệ mới: Bedrock, pgvector, WaveSurfer.js, ECS Fargate trong thời gian thực tập | ✅ | ☐ | ☐ |
| 3 | **Tính chủ động** | Chủ động phát hiện và giải quyết vấn đề kỹ thuật (throttling, timeout, version conflicts); đề xuất thay đổi kiến trúc | ✅ | ☐ | ☐ |
| 4 | **Tinh thần trách nhiệm** | Chịu trách nhiệm toàn bộ module AI pipeline; theo dõi tiến độ qua GitHub Issues; đúng deadline sprint | ✅ | ☐ | ☐ |
| 5 | **Kỷ luật** | Duy trì cập nhật tiến độ hàng ngày; tuân theo Git branching conventions và quy trình code review | ☐ | ✅ | ☐ |
| 6 | **Tư duy tiến bộ** | Tiếp nhận feedback từ admin FCAJ; ngay lập tức áp dụng cải tiến kiến trúc sau các buổi review | ✅ | ☐ | ☐ |
| 7 | **Giao tiếp** | Tham gia Sprint planning và họp nhóm; trình bày phát hiện kỹ thuật rõ ràng; cần cải thiện giao tiếp văn bản async | ☐ | ✅ | ☐ |
| 8 | **Làm việc nhóm** | Phối hợp API contract với team frontend; hỗ trợ đồng đội khi thay đổi backend ảnh hưởng đến công việc của họ | ✅ | ☐ | ☐ |
| 9 | **Tác phong chuyên nghiệp** | Tôn trọng quy tắc nhóm, đúng deadline, duy trì codebase gọn gàng cho đồng đội | ✅ | ☐ | ☐ |
| 10 | **Kỹ năng giải quyết vấn đề** | Chẩn đoán và giải quyết Bedrock throttling (Issue #72), Docker networking, version conflicts; áp dụng quy trình ADR | ✅ | ☐ | ☐ |
| 11 | **Đóng góp cho dự án/nhóm** | Xây dựng toàn bộ AI pipeline từ đầu; tạo nên tính năng semantic search cốt lõi của sản phẩm | ✅ | ☐ | ☐ |
| 12 | **Tổng thể** | Đầu ra kỹ thuật tốt; phát triển đáng kể về AWS cloud engineering và thiết kế hệ thống AI | ✅ | ☐ | ☐ |

### Điểm cần cải thiện

* Giao tiếp văn bản async: Cần ghi lại quyết định thiết kế và blockers chủ động hơn trên GitHub Issues thay vì chỉ trao đổi miệng trong buổi họp.
* Ước tính thời gian: Đánh giá sai về độ phức tạp của tích hợp AWS Bedrock (throttling, IAM permissions, VPC networking) nên cần thêm buffer time cho task liên quan cloud.
* Kỷ luật testing: Unit test và integration test được thêm muộn trong dự án; nên áp dụng TDD sớm hơn từ đầu sprint trong các dự án tới.
* Viết kỹ thuật tiếng Anh: Cải thiện khả năng viết tài liệu kỹ thuật rõ ràng, súc tích bằng tiếng Anh cho cộng tác nhóm quốc tế.