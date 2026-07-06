---
title: "Worklog Tuần 10"
date: 2026-06-22
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10 (22/06 – 28/06/2026):

* Tích hợp Dashboard với backend API thực tế (thay mock data bằng live data).
* Hoàn thiện tính năng Asset Detail: API integration, Video Streaming, In-Video Search.
* Thêm Audio Waveform visualization với WaveSurfer.js.
* Nâng cấp Upload flow, xử lý WebSocket, và In-Video Search phía backend + frontend.

### Các công việc triển khai trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | **Dashboard ↔ Backend API:** Kết nối Dashboard với real API — đồng bộ trạng thái asset real-time (polling + WebSocket fallback) | 22/06/2026 | 22/06/2026 | |
| 3   | **Asset Detail — Video Streaming:** Implement `GET /api/v1/assets/{id}/stream` hỗ trợ byte-range để phát video mượt | 23/06/2026 | 23/06/2026 | FastAPI StreamingResponse |
| 3   | **In-Video Search (Backend):** `POST /api/v1/assets/{id}/search` — embed query → pgvector similarity → trả về scenes có timestamp | 23/06/2026 | 23/06/2026 | |
| 4   | **Audio Waveform:** Tích hợp WaveSurfer.js; đồng bộ vị trí phát với video seek events | 24/06/2026 | 24/06/2026 | WaveSurfer.js docs |
| 5   | **In-Video Search (Frontend):** Input tìm kiếm → highlight scenes khớp trong SceneTimeline; tự nhảy đến scene trong VideoPlayer | 25/06/2026 | 25/06/2026 | |
| 5   | **Nâng cấp Upload flow:** Xử lý hủy upload, retry khi lỗi mạng, quản lý concurrent upload queue | 25/06/2026 | 25/06/2026 | |
| 6   | **Nâng cấp WebSocket:** Heartbeat ping/pong; reconnection logic với exponential backoff phía client | 26/06/2026 | 26/06/2026 | |
| 7   | **Phiên feedback:** Nhận feedback kiến trúc từ admin FCAJ; tối ưu communication patterns giữa các service | 27/06/2026 | 28/06/2026 | |

### Kết quả đạt được tuần 10:

* **Dashboard kết nối real API:** Asset cards hiển thị trạng thái xử lý live (QUEUED → PROCESSING → READY → ERROR); tự refresh khi state thay đổi.
* **Video Streaming:** Byte-range streaming cho phép seek đến bất kỳ vị trí nào mà không cần buffer toàn bộ file.
* **In-Video Search end-to-end:** Nhập query ngôn ngữ tự nhiên → hệ thống highlight scenes liên quan trên timeline và nhảy đến kết quả đầu tiên.
* **WaveSurfer.js audio waveform:** Waveform trực quan đồng bộ với video; click waveform seek cả audio và video.
* **Upload cải thiện:** Retry queue, hủy upload, upload đồng thời với progress tracking riêng từng file.
* **WebSocket đáng tin cậy:** Heartbeat giữ kết nối sống; client tự reconnect với exponential backoff.
* **Feedback FCAJ đã tích hợp:** Đơn giản hóa giao tiếp inter-service; loại bỏ API hop thừa; cải thiện error propagation lên frontend.
