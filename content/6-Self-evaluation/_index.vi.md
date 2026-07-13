---
title: "Tự đánh giá"
date: 2026-07-05
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

Trong thời gian thực tập tại First Cloud AI Journey (FCAJ) từ **17/04/2026** đến **17/07/2026** với vị trí Cloud/AI Engineering, tôi đã thực hiện dự án **Xây dựng hệ thống Smart Media Analytics trên nền tảng AWS**.

Mục tiêu của dự án là xây dựng một nền tảng phân tích video toàn diện kết hợp Generative AI và kiến trúc Serverless / Event-Driven trên AWS, hỗ trợ tìm kiếm ngữ nghĩa (Semantic Search). Hệ thống được chia thành các phân vùng chính:

* **Frontend & Authentication:** Lưu trữ ứng dụng React trên AWS Amplify kết hợp Route 53, xác thực người dùng bằng Amazon Cognito, và định tuyến API qua API Gateway và ALB tới ECS Backend API (AWS Fargate).
* **Ingestion & Orchestration:** Tiếp nhận video tải lên Amazon S3, kích hoạt Event Notification qua SQS và EventBridge đến AWS Step Functions để điều phối quá trình xử lý.
* **AI Processing Pipeline & Persistence:** Sử dụng ECS AI Worker (Fargate) kết hợp Amazon Transcribe và Amazon Bedrock (Nova Lite & Titan Embeddings) để trích xuất văn bản và tạo vector nhúng. Trạng thái tiến trình được cập nhật qua Redis và WebSocket, kết quả được lưu trữ trong RDS PostgreSQL (pgvector).
* **CI/CD & Observability:** Tự động hóa kiểm thử và triển khai qua GitHub Actions (đẩy Docker Image lên ECR), đồng thời giám sát hệ thống bằng Amazon CloudWatch và AWS X-Ray.

Thông qua dự án, tôi không chỉ áp dụng kiến thức về AI/ML mà còn có cơ hội thiết kế và triển khai một hệ thống đám mây hoàn chỉnh đáp ứng các tiêu chuẩn thực tế của doanh nghiệp.

Các nội dung chính đã thực hiện gồm:
* Thiết kế kiến trúc tổng thể hệ thống kết hợp Serverless, Container và Generative AI.
* Xây dựng pipeline tải lên và điều phối xử lý video tự động sử dụng S3, SQS, EventBridge và Step Functions.
* Triển khai module xử lý AI (scene detection, speech transcription, vision captioning, vector embedding) trên ECS AI Worker (Fargate).
* Tích hợp các dịch vụ AI nâng cao gồm Amazon Transcribe và Amazon Bedrock.
* Xây dựng cơ chế cập nhật trạng thái thời gian thực qua ElastiCache for Redis và kết nối WebSocket trên Application Load Balancer.
* Lưu trữ metadata và thực hiện Vector Search sử dụng extension pgvector trên Amazon RDS PostgreSQL.
* Viết tài liệu Workshop Website bằng Hugo, bao gồm các phần Proposal, Worklog, Workshop, Blog, Events, Self Evaluation và Feedback.
* Thiết lập luồng CI/CD tự động bằng GitHub Actions và triển khai giám sát qua CloudWatch và AWS X-Ray.
* Thực hiện kiểm thử và xây dựng quy trình dọn dẹp tài nguyên (Cleanup) nhằm tối ưu chi phí AWS.

Qua quá trình thực hiện, tôi tự đánh giá bản thân theo các tiêu chí sau:

| STT | Tiêu chí | Mức đánh giá | Nhận xét |
| --- | --- | --- | --- |
| 1 | Kiến thức chuyên môn | Tốt | Tôi đã vận dụng tốt kiến thức về AI/ML, Cloud Computing, FastAPI và React để xây dựng thành công hệ thống phân tích video hoàn chỉnh. |
| 2 | Khả năng học hỏi | Tốt | Tôi chủ động nghiên cứu và làm chủ các công nghệ mới như AWS Bedrock, pgvector, Step Functions và ECS Fargate trong suốt kỳ thực tập. |
| 3 | Tính chủ động | Tốt | Chủ động giải quyết các vấn đề kỹ thuật phức tạp như giới hạn băng thông (throttling), phân quyền IAM và cấu hình VPC mạng nội bộ. |
| 4 | Tinh thần trách nhiệm | Tốt | Tôi hoàn thành đầy đủ các nhiệm vụ của module AI Pipeline đúng thời hạn, đảm bảo mã nguồn hoạt động ổn định và có tài liệu hướng dẫn rõ ràng. |
| 5 | Kỷ luật trong công việc | Khá | Tuân thủ quy trình làm việc nhóm, quy ước đặt tên và nhánh Git. Tuy nhiên cần cải thiện khả năng quản lý thời gian để tối ưu hiệu năng làm việc. |
| 6 | Khả năng giao tiếp và trình bày | Tốt | Trình bày báo cáo rõ ràng, kết hợp sơ đồ kiến trúc trực quan giúp các thành viên khác dễ dàng tiếp cận và phối hợp dự án. |
| 7 | Khả năng làm việc nhóm / trao đổi | Khá | Phối hợp tốt với team frontend để định nghĩa các API contract, tuy nhiên cần chủ động giao tiếp văn bản (async communication) nhiều hơn trên GitHub Issues. |
| 8 | Tư duy giải quyết vấn đề | Tốt | Áp dụng tư duy phân tích hệ thống để chia nhỏ pipeline xử lý thành các tác vụ độc lập, giúp việc debug và tối ưu hóa hệ thống hiệu quả hơn. |
| 9 | Khả năng sử dụng công cụ | Tốt | Sử dụng thành thạo AWS Console/CLI, Docker, Git, Hugo và Markdown để hỗ trợ đắc lực cho công việc thiết kế và lập tài liệu. |
| 10 | Đóng góp vào dự án cá nhân | Tốt | Xây dựng hoàn chỉnh tính năng cốt lõi (AI pipeline và Semantic Search) cho sản phẩm Smart Media Analytics của nhóm. |
| 11 | Khả năng tự đánh giá và cải thiện | Tốt | Thường xuyên rà soát lại kiến trúc hệ thống, chủ động tiếp nhận feedback từ Admin FCAJ để tối ưu mã nguồn và hoàn thiện tài liệu báo cáo. |
| 12 | Đánh giá tổng thể | Tốt | Hoàn thành xuất sắc mục tiêu thực tập, nắm vững quy trình phát triển và vận hành một ứng dụng AI/ML thực tế trên nền tảng điện toán đám mây. |

### Điểm mạnh đạt được
* Hiểu sâu sắc quy trình phát triển, tích hợp và triển khai hệ thống AI/ML trên hạ tầng cloud AWS.
* Thành thạo trong việc phối hợp các dịch vụ Serverless (Lambda, Step Functions, SQS, API Gateway) và Container (ECS Fargate) để xử lý tác vụ nặng.
* Làm chủ việc tích hợp Generative AI qua Amazon Bedrock (Nova Lite, Titan Embeddings) và tìm kiếm ngữ nghĩa với pgvector trên RDS PostgreSQL.
* Xây dựng được luồng cập nhật thời gian thực (Real-time updates) sử dụng Redis Pub/Sub và WebSocket.
* Có kinh nghiệm thực tế về tự động hóa CI/CD bằng GitHub Actions và giám sát hệ thống với AWS CloudWatch, X-Ray.

### Điểm cần cải thiện
* Nâng cao kỹ năng kiểm thử tự động (Unit Test và Integration Test) ngay từ đầu chu kỳ phát triển sản phẩm (TDD).
* Tiếp tục tìm hiểu sâu hơn các giải pháp tối ưu chi phí hạ tầng, đặc biệt là chi phí liên quan đến Amazon Bedrock và các cụm ECS Fargate chạy liên tục.
* Tăng cường tính chủ động trong việc viết tài liệu thiết kế hệ thống và ghi nhận blockers trên các kênh trao đổi chung của dự án.
* Rèn luyện kỹ năng viết tài liệu kỹ thuật bằng tiếng Anh súc tích và chuyên nghiệp hơn.

### Kế hoạch phát triển trong thời gian tới
* Nghiên cứu và áp dụng mô hình MLOps để tự động hóa hoàn toàn quy trình cập nhật, đánh giá và deploy lại mô hình khi có dữ liệu mới.
* Tìm hiểu thêm về Infrastructure as Code (IaC) như AWS CDK hoặc Terraform để tự động hóa việc khởi tạo hạ tầng dự án.
* Thử nghiệm thêm các mô hình ngôn ngữ lớn (LLM) và kỹ thuật RAG tiên tiến trên Amazon Bedrock để tăng độ chính xác trong phân tích video.
* Nâng cao trình độ tiếng Anh chuyên ngành công nghệ thông tin phục vụ cho việc nghiên cứu tài liệu kỹ thuật quốc tế.

### Kết luận tự đánh giá
Qua kỳ thực tập tại FCAJ với dự án Smart Media Analytics, tôi đã có cơ hội quý báu để chuyển hóa kiến thức lý thuyết về AI và Cloud thành một sản phẩm thực tế có tính ứng dụng cao. Dự án giúp tôi củng cố vững chắc kỹ năng Cloud Engineering và AI Engineering, đồng thời rèn luyện phong cách làm việc chuyên nghiệp trong môi trường dự án.

Tôi tự đánh giá kết quả thực tập đạt mức **Tốt** và tự tin rằng những kinh nghiệm thực tế này sẽ là bệ phóng vững chắc cho sự nghiệp của tôi trong lĩnh vực Cloud và AI/ML Ops trong tương lai.