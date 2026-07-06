---
title: "Worklog Tuần 8"
date: 2026-06-08
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8 (08/06 – 14/06/2026):

* Tối ưu AI pipeline: chuyển đổi ASR model, cập nhật embedding model.
* Thiết kế Dashboard UI components theo Figma.
* Thống nhất kiến trúc AWS cloud và vẽ sơ đồ hệ thống.

### Các công việc triển khai trong tuần:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | **ASR Migration:** Chuyển từ WhisperX sang OpenAI Whisper (base) để ổn định hơn; cập nhật pipeline | 08/06/2026 | 08/06/2026 | ADR log |
| 3   | **Benchmark v2:** Chạy lại pipeline sau khi đổi ASR; 17s.MOV cải thiện từ 115.31s → **57.45s** (~50% nhanh hơn) | 09/06/2026 | 09/06/2026 | |
| 4   | Cập nhật cấu hình Embedding model; kiểm tra chất lượng output bge-m3 1024-dim với text tiếng Việt mẫu | 10/06/2026 | 10/06/2026 | |
| 5   | Thiết kế **Dashboard UI** components trong Figma: Video Card, Status Badge, Search Bar, Tag Filter, Pagination | 11/06/2026 | 11/06/2026 | Figma: Visualization-Of-SMA |
| 6   | Thiết kế **Upload Page** UI trong Figma: drag-and-drop zone, progress bar, file metadata preview | 12/06/2026 | 12/06/2026 | Figma: Visualization-Of-SMA |
| 7   | Thiết kế wireframe **Login/Register** và **Asset Detail** page trong Figma | 13/06/2026 | 13/06/2026 | Figma: Visualization-Of-SMA |
| CN  | Thống nhất kiến trúc AWS cloud: hoàn thiện service mapping (Local ↔ AWS); vẽ architecture diagram bằng draw.io | 14/06/2026 | 14/06/2026 | |

### Kết quả đạt được tuần 8:

* **ASR ổn định:** Chuyển sang OpenAI Whisper (base) — tương thích tốt hơn, không xung đột dependency.
* **Pipeline v2 benchmark:** Thời gian xử lý 17s.MOV giảm một nửa (115.31s → 57.45s); tổng 3 file: **972.09s** (−2.5% so v1).
* Hoàn thành 4 màn hình Figma: Dashboard, Upload, Login/Register, Asset Detail — design system nhất quán với dark theme.
* Sơ đồ kiến trúc AWS hoàn thiện: luồng 14 bước từ user access đến kết quả vector search, bao phủ đầy đủ các dịch vụ AWS.
* Xác nhận quyết định kiến trúc: mapping Local (Docker) ↔ AWS cho 10 components được ghi lại trong bảng ADR.
