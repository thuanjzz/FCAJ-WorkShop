---
title: "Worklog Tuần 9"
date: 2026-06-15
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9 (15/06 – 21/06/2026):

* Implement Asset Detail Page UI theo thiết kế Figma.
* Xây dựng Upload Video flow với WebSocket theo dõi tiến độ real-time.
* Chuyển AI provider từ Ollama local sang AWS Bedrock; chuyển vector store từ ChromaDB sang PostgreSQL + pgvector.

### Các công việc triển khai trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | **Asset Detail Page:** Xây dựng React components — VideoPlayer, TranscriptPanel, SceneTimeline, InsightsCard (theo Figma) | 15/06/2026 | 16/06/2026 | Figma design |
| 4   | **Upload Flow + WebSocket:** Implement multipart upload → S3 presigned URL; WebSocket endpoint real-time job progress | 17/06/2026 | 17/06/2026 | FastAPI WebSocket docs |
| 5   | Thêm multi-process cho upload: xử lý nhiều file đồng thời; state recovery khi reconnect | 18/06/2026 | 18/06/2026 | |
| 6   | Xây dựng API `POST /api/v1/assets/{id}/reingest` và `POST /api/v1/assets/{id}/regenerate-insights` | 19/06/2026 | 19/06/2026 | |
| 6   | **AI Provider abstraction:** Refactor pipeline dùng provider interface — swap Ollama ↔ Bedrock không cần sửa pipeline code | 19/06/2026 | 19/06/2026 | boto3 docs |
| 7   | **Tích hợp AWS Bedrock:** Implement `BedrockNovaLiteProvider` (captioning) và `BedrockTitanEmbeddingsProvider` (1024-dim vectors) | 20/06/2026 | 20/06/2026 | AWS Bedrock docs |
| 7   | **Migration pgvector:** Thêm extension pgvector vào PostgreSQL; chuyển vector storage từ ChromaDB sang RDS PostgreSQL | 20/06/2026 | 20/06/2026 | pgvector docs |
| CN  | **Điều chỉnh kiến trúc AWS:** Cập nhật diagram phản ánh thay đổi Bedrock + pgvector; review với mentor | 21/06/2026 | 21/06/2026 | |

### Kết quả đạt được tuần 9:

* **Asset Detail Page** hoàn thiện: VideoPlayer seek-to-scene, TranscriptPanel có timestamps, SceneTimeline, AI Insights panel — khớp sát thiết kế Figma.
* **Upload + WebSocket:** Người dùng thấy tiến độ real-time (VD: "Đang chuyển lời... 45%") khi pipeline chạy; state phục hồi khi làm mới browser.
* **Re-Ingest & Regenerate APIs:** Cho phép tái xử lý video hoặc tái tạo AI insights mà không cần upload lại.
* **AI Provider abstraction layer** (`AIProvider` interface): chuyển đổi giữa Ollama (local) và Bedrock (cloud) chỉ qua cấu hình.
* **AWS Bedrock tích hợp thành công:** Bedrock Amazon Nova Lite cho captioning; Bedrock Titan Embeddings v2 (1024-dim) — test thành công với keyframe thực tế.
* **ChromaDB → pgvector migration hoàn tất:** Toàn bộ vector data lưu trong PostgreSQL với cột `vector(1024)`; cosine similarity query hoạt động qua toán tử `<=>`.
* Sơ đồ kiến trúc AWS cập nhật phản ánh stack production cuối cùng.
