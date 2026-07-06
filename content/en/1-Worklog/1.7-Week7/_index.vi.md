---
title: "Worklog Tuần 7"
date: 2026-06-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7 (01/06 – 07/06/2026):

* Hoàn thiện AI pipeline đầy đủ: Scene Detection → Transcription → Captioning.
* Tích hợp ChromaDB làm vector store cho semantic search.
* Expose Search API và Ingest API dưới dạng async FastAPI endpoints.

### Các công việc triển khai trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | Hoàn thiện module **Scene Detection**: PySceneDetect + OpenCV, threshold tuning, trích keyframe ra disk | 01/06/2026 | 01/06/2026 | PySceneDetect docs |
| 3   | Hoàn thiện module **Transcription**: faster-whisper (int8, CPU); output segment-level với timestamps; phát hiện ngôn ngữ | 02/06/2026 | 02/06/2026 | faster-whisper GitHub |
| 4   | Hoàn thiện module **Captioning**: Ollama qwen2.5-vl:3b; resize keyframe về 720p trước inference; output JSON có cấu trúc | 03/06/2026 | 03/06/2026 | Ollama docs |
| 5   | Tích hợp **ChromaDB**: lưu text embeddings (bge-m3, 1024-dim) từ captions + transcripts; collection riêng mỗi asset | 04/06/2026 | 04/06/2026 | ChromaDB docs |
| 6   | Xây dựng **Ingest API** (`POST /api/v1/ingest`): async FastAPI + BackgroundTasks; pipeline chạy tuần tự trong background | 05/06/2026 | 05/06/2026 | FastAPI docs |
| 7   | Xây dựng **Search API** (`POST /api/v1/search`): embed query → ChromaDB cosine similarity → trả về danh sách scenes có timestamp | 06/06/2026 | 06/06/2026 | |
| CN  | **Benchmark v1:** Chạy full pipeline trên 3 file test; tổng thời gian 997.55s; xác định bottleneck | 07/06/2026 | 07/06/2026 | |

### Kết quả đạt được tuần 7:

* **AI pipeline hoạt động đầy đủ** end-to-end: upload video → detect scenes → transcribe → caption → embed → store → search.
* Scene Detection: chia video 5 phút thành các cảnh có nghĩa dùng content-aware detection.
* Transcription (faster-whisper int8): nhận dạng giọng nói tiếng Việt chính xác; timestamps theo segment được giữ lại.
* Captioning (qwen2.5-vl:3b): sinh caption mô tả cảnh; JSON có cấu trúc với objects, actions, emotions.
* ChromaDB: lưu bge-m3 1024-dim embeddings theo scene; cosine similarity search hoạt động.
* Ingest API trả về `202 Accepted` ngay lập tức; pipeline chạy background — non-blocking.
* Search API trả về các scene ngữ nghĩa liên quan với truy vấn ngôn ngữ tự nhiên (test tiếng Việt và tiếng Anh).
* **Benchmark v1 baseline:** 17s.MOV = 115.31s | 1p58s.MOV = 91.91s | 5p08s.MOV = 790.20s | Tổng = **997.55s**.
