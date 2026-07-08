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
| 3   | **Video Streaming & In-Video Search (Backend):** Implement streaming hỗ trợ byte-range và API search trong video (pgvector similarity) | 23/06/2026 | 23/06/2026 | FastAPI StreamingResponse |
| 4   | **Audio Waveform:** Tích hợp WaveSurfer.js; đồng bộ vị trí phát với video seek events | 24/06/2026 | 24/06/2026 | WaveSurfer.js docs |
| 5   | **In-Video Search (Frontend) & Upload flow:** Tích hợp UI search tìm kiếm trong video và nâng cấp tính năng upload concurrent queue | 25/06/2026 | 25/06/2026 | |
| 6   | **Nâng cấp WebSocket:** Heartbeat ping/pong; reconnection logic với exponential backoff phía client | 26/06/2026 | 26/06/2026 | |
| 7   | **Phiên feedback:** Nhận feedback kiến trúc từ admin FCAJ; thảo luận định hướng tối ưu hóa hệ thống | 27/06/2026 | 27/06/2026 | |
| CN  | **Tối ưu kiến trúc:** Tối ưu hóa communication patterns giữa các service dựa trên feedback | 28/06/2026 | 28/06/2026 | |

### Kết quả đạt được tuần 10:

* **Dashboard kết nối real API:** Asset cards hiển thị trạng thái xử lý live (QUEUED → PROCESSING → READY → ERROR); tự refresh khi state thay đổi.
* **Video Streaming:** Byte-range streaming cho phép seek đến bất kỳ vị trí nào mà không cần buffer toàn bộ file.
* **In-Video Search end-to-end:** Nhập query ngôn ngữ tự nhiên → hệ thống highlight scenes liên quan trên timeline và nhảy đến kết quả đầu tiên.
* **WaveSurfer.js audio waveform:** Waveform trực quan đồng bộ với video; click waveform seek cả audio và video.
* **Upload cải thiện:** Retry queue, hủy upload, upload đồng thời với progress tracking riêng từng file.
* **WebSocket đáng tin cậy:** Heartbeat giữ kết nối sống; client tự reconnect với exponential backoff.
* **Feedback FCAJ đã tích hợp:** Đơn giản hóa giao tiếp inter-service; loại bỏ API hop thừa; cải thiện error propagation lên frontend.
