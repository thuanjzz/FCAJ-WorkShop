---
title: "Tự động số hóa hồ sơ bệnh án với Amazon Bedrock Data Automation và AWS HealthLake"
date: 2026-07-05
weight: 2
chapter: false
---

Hôm nay mình muốn chia sẻ một kiến trúc khá thú vị từ AWS dành cho lĩnh vực y tế: **tự động số hóa hồ sơ bệnh án bằng AI với Amazon Bedrock Data Automation kết hợp AWS HealthLake**.

Trong quá trình chuyển đổi số, rất nhiều bệnh viện vẫn đang lưu trữ bệnh án dưới dạng giấy hoặc PDF scan. Điều này khiến việc tìm kiếm thông tin, tổng hợp lịch sử điều trị hay chia sẻ dữ liệu giữa các hệ thống mất khá nhiều thời gian. Nếu nhập liệu thủ công thì vừa tốn nhân lực, vừa dễ xảy ra sai sót.

Bài toán đặt ra là: làm thế nào để tự động chuyển các tài liệu y tế không có cấu trúc thành dữ liệu số có thể tìm kiếm, phân tích và tích hợp với các hệ thống quản lý bệnh viện?

AWS đưa ra một kiến trúc serverless kết hợp AI để giải quyết bài toán này.

### Kiến trúc hoạt động như thế nào?

Toàn bộ quy trình được kích hoạt tự động ngay khi hồ sơ được tải lên Amazon S3.

![Kiến trúc hoạt động](/images/healthlake_architecture.png)

Luồng xử lý gồm các bước:

1.  **Bước 1 - Upload hồ sơ:** Người dùng (nhân viên y tế) upload hồ sơ bệnh án (PDF hoặc hình ảnh) lên Amazon S3.
2.  **Bước 2 - S3 Event Notification:** S3 gửi sự kiện `PutObject` kích hoạt Lambda function (BDA Job Trigger).
3.  **Bước 3 - BDA Job Trigger:** Lambda function nhận sự kiện và khởi động tiến trình xử lý trong Amazon Bedrock Data Automation.
4.  **Bước 4 - Phân tích dữ liệu:** Amazon Bedrock Data Automation phân tích tài liệu bằng BDA Blueprints (JSON templates) và tự động trích xuất các thông tin quan trọng như: thông tin bệnh nhân, chẩn đoán, kết quả xét nghiệm, đơn thuốc, và dấu hiệu sinh tồn.
5.  **Bước 5 - Lưu kết quả:** Dữ liệu trích xuất từ BDA được lưu trữ trong một bucket Amazon S3 khác.
6.  **Bước 6 - Chuyển đổi FHIR:** Một Lambda function khác nhận sự kiện lưu file mới, chuyển đổi dữ liệu y tế này sang chuẩn FHIR R4 và tạo tiến trình import API vào HealthLake.
7.  **Bước 7 - Lưu trữ tại HealthLake:** AWS HealthLake lưu trữ và lập chỉ mục cho dữ liệu FHIR.
8.  **Bước 8 - Truy xuất dữ liệu:** Các hệ thống y tế hoặc ứng dụng máy khách truy vấn dữ liệu lâm sàng một cách bảo mật qua chuẩn FHIR APIs.

Toàn bộ nhật ký hoạt động được gửi đến Amazon CloudWatch để giám sát và ghi log tập trung. Điểm hay là toàn bộ quy trình hoạt động theo mô hình event-driven, nghĩa là chỉ xử lý khi có tài liệu mới được tải lên, không cần duy trì máy chủ chạy liên tục.

### Amazon Bedrock Data Automation có gì đặc biệt?

Điểm nổi bật nhất là không cần tự xây dựng pipeline OCR và NLP riêng.

Thông thường để xử lý hồ sơ bệnh án, doanh nghiệp sẽ phải kết hợp nhiều bước:
*   OCR (Nhận dạng ký tự quang học)
*   Phân tích bố cục (Layout Analysis)
*   Trích xuất thực thể (Entity Extraction)
*   Chuẩn hóa dữ liệu
*   Mapping sang chuẩn FHIR

Giờ đây **Amazon Bedrock Data Automation** giúp tự động hóa phần lớn quy trình này, giảm đáng kể công sức phát triển và bảo trì. Đây là hướng tiếp cận rất phù hợp với các tổ chức muốn nhanh chóng triển khai AI mà không cần xây dựng mô hình từ đầu.

### Use Case 1: Số hóa bệnh án cũ

Một bệnh viện có thể có hàng triệu hồ sơ bệnh án giấy được lưu trữ nhiều năm. Thay vì phải nhập liệu thủ công, toàn bộ tài liệu có thể được scan và upload lên Amazon S3.

Pipeline sẽ tự động:
*   Trích xuất dữ liệu
*   Chuẩn hóa
*   Lưu vào AWS HealthLake

Sau đó bác sĩ chỉ cần tìm kiếm theo tên bệnh nhân, mã bệnh hoặc lịch sử điều trị thay vì mở từng tập hồ sơ giấy.

### Use Case 2: Hỗ trợ AI phân tích hồ sơ bệnh nhân

Khi dữ liệu đã được chuẩn hóa theo FHIR, việc xây dựng các ứng dụng AI hạ nguồn sẽ trở nên dễ dàng hơn rất nhiều.
Ví dụ:
*   AI tóm tắt lịch sử điều trị của bệnh nhân.
*   Chatbot hỗ trợ bác sĩ tra cứu bệnh án nhanh chóng.
*   Phân tích xu hướng điều trị và hỗ trợ nghiên cứu lâm sàng.
*   Kết nối dữ liệu điều trị liên thông giữa nhiều bệnh viện.

Đây cũng là nền tảng quan trọng để phát triển các hệ thống Generative AI trong lĩnh vực y tế.

### Vì sao AWS sử dụng HealthLake?

Điểm mình đánh giá cao là AWS không chỉ tập trung vào việc "đọc được tài liệu", mà còn chuẩn hóa dữ liệu theo FHIR (Fast Healthcare Interoperability Resources) — tiêu chuẩn trao đổi dữ liệu y tế được sử dụng rộng rãi trên thế giới.

Điều này giúp dữ liệu sau khi xử lý không bị "khóa" trong một ứng dụng riêng, mà có thể tích hợp dễ dàng với nhiều hệ thống HIS, EMR hoặc các ứng dụng AI khác.

### Một vài lưu ý khi triển khai

AWS cũng lưu ý rằng kiến trúc trong bài viết được xây dựng để minh họa. Nếu triển khai trên dữ liệu bệnh nhân thật, cần bổ sung thêm các yêu cầu về bảo mật như:
*   Mã hóa dữ liệu khi lưu trữ và truyền tải.
*   Quản lý quyền truy cập bằng IAM theo nguyên tắc quyền tối thiểu.
*   Theo dõi hoạt động của hệ thống bằng CloudTrail.
*   Triển khai trong môi trường đáp ứng các yêu cầu tuân thủ như HIPAA (nếu áp dụng).

### Tổng kết

Theo mình, điều đáng giá nhất của kiến trúc này không nằm ở AI hay OCR riêng lẻ, mà là khả năng tự động chuyển đổi tài liệu y tế không có cấu trúc thành dữ liệu chuẩn hóa, sẵn sàng cho các hệ thống và ứng dụng AI sử dụng. Đây là một ví dụ điển hình cho cách AWS kết hợp Serverless + AI + Chuẩn dữ liệu ngành để giải quyết một bài toán rất thực tế trong lĩnh vực chăm sóc sức khỏe.

***

*   **Tham khảo từ AWS Architecture Blog:** [Automate medical record digitization with Amazon Bedrock Data Automation and AWS HealthLake](https://aws.amazon.com/blogs/architecture/automate-medical-record-digitization-with-amazon-bedrock-data-automation-and-aws-healthlake/)
