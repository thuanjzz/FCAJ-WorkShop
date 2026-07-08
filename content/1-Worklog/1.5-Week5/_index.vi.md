---
title: "Worklog Tuần 5"
date: 2026-05-18
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5 (18/05 – 24/05/2026):

* Khởi động **Sprint 1** của dự án `smart_media_analytics_cloudforge`.
* Xác định API Data Contract và schema cơ sở dữ liệu giữa frontend và backend.
* Dựng môi trường Docker Compose: frontend, backend, ChromaDB.
* Xây dựng AI pipeline cơ bản và FastAPI backend hoạt động được.

### Các công việc triển khai trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | Sprint 1 planning: xác định user stories, phân chia task, thiết lập GitHub Project board | 18/05/2026 | 18/05/2026 | GitHub Issues Board |
| 3   | Thiết kế API Data Contract: OpenAPI spec cho Ingest API, Search API; định nghĩa request/response schemas | 19/05/2026 | 19/05/2026 | |
| 4   | Thiết kế PostgreSQL schema: bảng `assets`, `scenes`, `transcripts`, `embeddings`; vector column với pgvector | 20/05/2026 | 20/05/2026 | |
| 5   | Cài đặt Docker Compose: FastAPI + Uvicorn, PostgreSQL 16, ChromaDB, Redis; cấu hình biến môi trường | 21/05/2026 | 21/05/2026 | |
| 6   | Xây dựng FastAPI backend cơ bản: health check, file upload endpoint (multipart), kết nối PostgreSQL qua SQLAlchemy | 22/05/2026 | 22/05/2026 | |
| 7   | Xây dựng AI pipeline cơ bản (local): FFmpeg scene detection → faster-whisper transcription → Ollama captioning | 23/05/2026 | 23/05/2026 | |
| CN  | Khởi tạo React 19 + Vite + Tailwind CSS frontend; routing cơ bản (Upload, Dashboard); kết nối backend API | 24/05/2026 | 24/05/2026 | |

### Kết quả đạt được tuần 5:

* Hoàn thành Sprint 1 planning với 15+ user stories trên GitHub Issues; thiết lập Agile workflow với labels và milestones.
* Xác định API Data Contract (OpenAPI 3.0 spec) — thỏa thuận giao diện rõ ràng giữa frontend và backend.
* Thiết kế schema PostgreSQL: bảng `assets`, `scenes`, `transcripts`, `tags` với indexes và foreign keys đầy đủ.
* Docker Compose hoạt động thành công: FastAPI (:8000), PostgreSQL (:5432), ChromaDB (:8001), Redis (:6379).
* FastAPI backend phục vụ: `POST /api/v1/ingest` (file upload), `GET /api/v1/health`, `GET /api/v1/assets`.
* AI pipeline prototype cơ bản: FFmpeg trích keyframes → faster-whisper transcript audio → Ollama (qwen2.5-vl:3b) sinh caption.
* React frontend khởi tạo với Vite; Tailwind CSS cấu hình; routing Upload và Dashboard hoạt động.
