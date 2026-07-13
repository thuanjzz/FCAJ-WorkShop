---
title: "Chia sẻ và Đóng góp ý kiến"
date: 2026-07-05
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

Trong suốt quá trình thực tập và xây dựng báo cáo dưới hình thức Workshop Website, tôi không chỉ có cơ hội tiếp cận các dịch vụ điện toán đám mây của AWS mà còn học được cách trình bày và chia sẻ một dự án kỹ thuật một cách có hệ thống. So với việc viết báo cáo truyền thống, việc xây dựng một website yêu cầu tôi phải tổ chức nội dung hợp lý, diễn giải các bước triển khai rõ ràng và đảm bảo người đọc có thể dễ dàng theo dõi cũng như tái hiện lại quy trình thực hiện.

Dự án tôi lựa chọn là **Smart Media Analytics** (thuộc nhóm **CloudForge**). Đây là đề tài giúp tôi kết nối nhiều kiến thức đã học như xử lý dữ liệu, Generative AI, kiến trúc Cloud, xử lý dữ liệu thời gian thực, giám sát hệ thống và tối ưu chi phí vận hành.

### 1. Cảm nhận về chương trình thực tập
Điều tôi đánh giá cao nhất ở chương trình thực tập tại First Cloud AI Journey (FCAJ) là định hướng học tập thông qua một dự án thực tế. Thay vì chỉ tìm hiểu từng dịch vụ AWS riêng lẻ, tôi được khuyến khích kết hợp nhiều dịch vụ để xây dựng một hệ thống hoàn chỉnh phục vụ cho một bài toán cụ thể.

Trong dự án Smart Media Analytics, mỗi dịch vụ AWS đảm nhận một vai trò riêng:
* **AWS Amplify & Route 53:** Lưu trữ frontend React và quản lý tên miền.
* **Amazon Cognito:** Đảm nhận việc đăng ký, đăng nhập và bảo mật thông tin người dùng.
* **Amazon S3:** Tiếp nhận video upload an toàn từ người dùng.
* **Amazon SQS & EventBridge:** Nhận diện sự kiện và kích hoạt pipeline xử lý.
* **AWS Step Functions:** Đóng vai trò nhạc trưởng điều phối luồng xử lý video tự động.
* **Amazon ECS (AWS Fargate):** Host các Backend API và các Worker xử lý tác vụ AI nặng.
* **Amazon Bedrock & Amazon Transcribe:** Dịch vụ AI managed phục vụ Speech-to-Text và tạo vector nhúng (Embeddings).
* **ElastiCache for Redis:** Cập nhật trạng thái xử lý thời gian thực qua WebSocket.
* **Amazon RDS PostgreSQL (pgvector):** Lưu trữ metadata và thực hiện Vector Search.
* **Amazon CloudWatch & AWS X-Ray:** Theo dõi log và giám sát hoạt động thời gian thực của hệ thống.

Quá trình kết hợp các dịch vụ này giúp tôi hiểu rõ hơn cách xây dựng một kiến trúc Cloud hoàn chỉnh thay vì chỉ sử dụng từng dịch vụ một cách độc lập.

### 2. Quá trình học tập và làm việc
Trong thời gian thực tập, tôi có cơ hội rèn luyện khả năng tự học và chủ động tìm hiểu tài liệu. Nhiều khái niệm như IAM Role, cấu hình ECS Task, pgvector semantic search, suy luận Generative AI thời gian thực hay cách Hugo chuyển đổi Markdown thành website ban đầu đều khá mới đối với tôi.

Thay vì chỉ làm theo hướng dẫn có sẵn, tôi chủ động đọc tài liệu AWS, thử nghiệm nhiều cách triển khai khác nhau và ghi chép lại những kiến thức quan trọng để bổ sung vào báo cáo.

Trong quá trình thực hiện, tôi luôn đặt ra các câu hỏi như:
* Dữ liệu được truyền qua các dịch vụ AWS như thế nào?
* Vai trò của từng thành phần trong hệ thống là gì?
* Những bước nào cần minh họa bằng hình ảnh?
* Những tài nguyên AWS nào cần được dọn dẹp sau khi kiểm thử?
* Làm thế nào để người khác có thể dễ dàng thực hiện lại dự án?

Việc tự trả lời những câu hỏi này giúp tôi hiểu sâu hơn về bản chất của hệ thống thay vì chỉ mô tả các dịch vụ AWS.

### 3. Mối liên hệ với chuyên ngành
Dự án có sự gắn kết chặt chẽ với định hướng học tập của tôi trong lĩnh vực Khoa học dữ liệu (Data Science), Machine Learning và Cloud Engineering.

Trước đây, khi học Machine Learning và AI, tôi thường tập trung vào việc xây dựng mô hình, huấn luyện và đánh giá độ chính xác. Tuy nhiên, thông qua dự án này, tôi nhận ra rằng để một mô hình có thể hoạt động trong môi trường thực tế cần rất nhiều thành phần hỗ trợ như lưu trữ dữ liệu, thiết kế API, truyền thông tin thời gian thực, giám sát, ghi log, bảo mật và quản lý tài nguyên.

Điều này giúp tôi có cái nhìn toàn diện hơn về vòng đời của một hệ thống AI khi triển khai trên nền tảng điện toán đám mây (MLOps/LMOps).

### 4. Kết quả đạt được
Điều tôi hài lòng nhất là đã xây dựng được một nội dung mang dấu ấn cá nhân thay vì chỉ sử dụng nguyên mẫu của workshop.

Tôi đã điều chỉnh lại cấu trúc báo cáo để phù hợp với dự án Smart Media Analytics, đồng thời loại bỏ hoặc thay thế những nội dung không còn phù hợp với mục tiêu của mình.

Báo cáo được tổ chức thành các phần rõ ràng gồm:
* Thông tin sinh viên.
* Worklog theo từng tuần.
* Đề xuất dự án (Proposal).
* Các bài viết Blog.
* Các sự kiện đã tham gia (Events).
* Workshop kỹ thuật.
* Tự đánh giá (Self Evaluation).
* Chia sẻ và đóng góp ý kiến (Feedback).

Việc chia nhỏ nội dung giúp tôi dễ dàng theo dõi tiến độ thực hiện cũng như đảm bảo báo cáo có bố cục logic và nhất quán.

### 5. Những khó khăn gặp phải

#### 5.1. Hiểu và mô tả đúng kiến trúc hệ thống
Một trong những khó khăn lớn nhất là nắm bắt mối liên kết giữa nhiều dịch vụ AWS trong cùng một hệ thống phân tán kết hợp AI.

Để nội dung dễ theo dõi hơn, tôi chia kiến trúc thành hai phần chính:
* **AI Processing Pipeline:** Gồm quá trình Ingestion từ S3, điều phối Step Functions, xử lý trích xuất văn bản (Transcribe), tạo vector nhúng (Bedrock) của ECS Worker và lưu trữ vào RDS PostgreSQL (pgvector).
* **Application API & Search Flow:** Gồm luồng frontend React kết nối qua API Gateway/ALB đến ECS Backend API để truy vấn dữ liệu và thực hiện tìm kiếm ngữ nghĩa thời gian thực.

Cách phân chia này giúp việc trình bày kiến trúc trực quan và dễ hiểu hơn.

#### 5.2. Xây dựng Website bằng Hugo
Việc tìm hiểu cách Hugo chuyển đổi các tệp Markdown thành website cũng là một thử thách ban đầu.

Thông qua quá trình thực hành, tôi hiểu rõ hơn về cấu trúc thư mục của Hugo, cách quản lý nội dung đa ngôn ngữ, cách tổ chức hình ảnh và sử dụng thư mục static.

Một lỗi tôi từng gặp là sử dụng sai định dạng đường dẫn hình ảnh trong Markdown hoặc viết sai tên thư mục do phân biệt chữ hoa chữ thường (ví dụ: `5.13-observability` thay vì `5.13-Observability`). Sau khi khắc phục, tôi hiểu rõ hơn cách Hugo xử lý các tài nguyên tĩnh khi xây dựng website.

#### 5.3. Lựa chọn hình ảnh minh họa
Một khó khăn khác là lựa chọn hình ảnh phù hợp cho từng bước triển khai. Nếu sử dụng quá nhiều hình ảnh, báo cáo sẽ trở nên dài và khó theo dõi; ngược lại, nếu thiếu hình ảnh ở những bước quan trọng thì người đọc sẽ khó hình dung quá trình thực hiện.

Vì vậy, tôi chỉ bổ sung hình ảnh tại những bước có giá trị minh chứng cao như:
* Sơ đồ kiến trúc nền tảng.
* Giao diện thiết lập Cognito, Amplify, và Route 53.
* Cấu hình ECS Task Role & IAM policies.
* Cấu trúc sidecar container xray-daemon và cài đặt cổng UDP 2000.
* Kết quả hiển thị Service Map trên AWS X-Ray Console.
* Log streams trên CloudWatch Logs.
* Quá trình dọn dẹp tài nguyên sau khi triển khai.

### 6. Một số đề xuất
Từ trải nghiệm của bản thân, tôi có một số đề xuất nhằm nâng cao chất lượng chương trình:
* Cung cấp checklist rõ ràng cho từng phần của báo cáo như Proposal, Worklog, Workshop và Self-evaluation.
* Bổ sung hướng dẫn chi tiết về cách tổ chức hình ảnh và quản lý thư mục trong Hugo để tránh các lỗi đường dẫn tĩnh.
* Tổ chức một buổi hướng dẫn ngắn về cách build và kiểm tra website Hugo cục bộ trước khi deploy/nộp.
* Khuyến khích sinh viên phân biệt rõ những nội dung đã triển khai thực tế, những phần mới dừng ở mức thiết kế lý thuyết và các nội dung cần tiếp tục nghiên cứu.
* Nhấn mạnh hơn về việc tối ưu chi phí và dọn dẹp tài nguyên AWS sau khi hoàn thành dự án để tránh phát sinh chi phí ngoài ý muốn.

### 7. Đánh giá chung
Tôi sẵn sàng giới thiệu chương trình thực tập này cho các bạn sinh viên quan tâm đến AWS, Cloud Computing hoặc AI/ML Engineering.

Chương trình không chỉ giúp người học tiếp cận công nghệ mới nhất như Generative AI (Amazon Bedrock) mà còn tạo cơ hội xây dựng một dự án hoàn chỉnh có thể sử dụng trong báo cáo thực tập hoặc làm giàu portfolio cá nhân.

Mặc dù yêu cầu khả năng tự học khá cao, nhưng chính quá trình tự nghiên cứu và giải quyết vấn đề thực tế (ví dụ: xử lý lỗi Bedrock API throttling) đã giúp tôi tích lũy được nhiều kinh nghiệm thực tiễn quý báu.

### 8. Định hướng phát triển
Sau khi hoàn thành chương trình, tôi mong muốn tiếp tục phát triển hệ thống Smart Media Analytics theo các hướng sau:
* Áp dụng mô hình MLOps/LMOps để tự động hóa quy trình huấn luyện lại hoặc cập nhật mô hình embeddings khi có dữ liệu mới.
* Tự động hóa quá trình triển khai hạ tầng AWS sử dụng Infrastructure as Code (IaC) như Terraform hoặc AWS CDK.
* Thử nghiệm thêm nhiều mô hình ngôn ngữ lớn (LLM) đa phương thức khác trên Amazon Bedrock nhằm tăng tốc độ và độ chính xác khi phân tích video.
* Tiếp tục hoàn thiện tài liệu kỹ thuật và đồng bộ nội dung đầy đủ giữa phiên bản tiếng Việt và tiếng Anh.

### 9. Kết luận
Quá trình thực tập giúp tôi hiểu rõ hơn cách chuyển đổi một ý tưởng AI/ML thành một hệ thống Cloud hoàn chỉnh có khả năng triển khai thực tế. Bên cạnh việc nâng cao kiến thức chuyên môn, tôi còn rèn luyện được kỹ năng xây dựng tài liệu kỹ thuật, trình bày quy trình triển khai và chia sẻ kiến thức thông qua một website.

Điều quan trọng nhất tôi học được là giá trị của một dự án Cloud không nằm ở số lượng dịch vụ AWS được sử dụng, mà ở cách kết hợp các dịch vụ đó để giải quyết hiệu quả một bài toán thực tế. Đây sẽ là nền tảng quan trọng để tôi tiếp tục học tập và phát triển trong lĩnh vực AI và Cloud Engineering trong tương lai.