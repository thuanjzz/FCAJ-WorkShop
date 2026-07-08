---
title: "Chia sẻ và Đóng góp ý kiến"
date: 2026-07-05
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

### Đánh giá tổng quan

**1. Môi trường làm việc**

Môi trường làm việc tại FCAJ rất cộng tác và kích thích tư duy kỹ thuật. Mentor và admin tạo điều kiện thảo luận trên Slack, cung cấp feedback kịp thời trong các buổi review và khuyến khích intern tự đưa ra quyết định kiến trúc thực sự thay vì chỉ làm theo hướng dẫn. Văn hóa async-first tôn trọng phong cách làm việc cá nhân, giúp tôi tập trung sâu vào các bài toán pipeline phức tạp. Nếu có thể đề xuất: thêm các buổi whiteboarding video call cho thảo luận kiến trúc sẽ đẩy nhanh sự đồng thuận của nhóm.

**2. Hỗ trợ từ Mentor / Cán bộ hướng dẫn**

Mentor Nguyễn Gia Hưng hướng dẫn ở mức độ phù hợp không cho câu trả lời trực tiếp mà hướng cho tôi đến các anh chị đi trước bên trong FCAJ hướng dẫn và trao đổi về tài liệu AWS cùng với việc đặt câu hỏi phù hợp để thúc đẩy tư duy phản biện từ các buổi hội thảo. Các buổi review kiến trúc của anh chị FCAJ ở Tuần 10 đặc biệt có giá trị đối với team của chúng tôi giúp chúng tôi hiểu và chỉnh sửa lại những phần kiến trúc bị sai và cải thiện tốt hơn.

**3. Mức độ liên quan đến ngành học**

Các công việc được giao phù hợp rất tốt với chuyên ngành Khoa Học Dữ Liệu của tôi. Tôi đồng thời ứng dụng các kiến thức về machine learning (embedding models, cosine similarity), kỹ thuật phần mềm (clean architecture, API design) và cloud computing (AWS managed services). Kỳ thực tập lấp đầy khoảng trống thực tiễn mà học đại học một mình không cung cấp được đặc biệt là thiết kế hệ thống production và tối ưu chi phí cloud.

**4. Cơ hội học hỏi & Phát triển kỹ năng**

11 tuần mang lại nhiều kinh nghiệm thực chiến AWS hơn tôi kỳ vọng:
- **Tuần 1–4:** Nền tảng AWS vững chắc (EC2, S3, Lambda, DynamoDB, API Gateway, VPC).
- **Tuần 5–11:** Phát triển hệ thống AI production thực sự — từ Docker Compose prototype đến AWS Fargate deployment.

Kỹ năng mới chính đạt được: AWS Bedrock API, cấu hình ECS Fargate task, pgvector semantic search, kiến trúc WebSocket real-time, và CI/CD với GitHub Actions → ECR.

**5. Văn hóa công ty & Tinh thần nhóm**

FCAJ vận hành như một team kỹ thuật chuyên nghiệp dù là chương trình đào tạo. Sprint planning, backlog grooming và code review được thực hiện nghiêm túc. Văn hóa ghi lại quyết định (ADR log, GitHub Issues) và đo lường tiến độ định lượng (benchmark results) trực tiếp cải thiện thói quen kỹ thuật của tôi. Nhóm ăn mừng thành công kỹ thuật và coi thất bại là cơ hội học hỏi văn hóa kỹ thuật lành mạnh mà tôi mong muốn mang vào sự nghiệp.

**6. Chính sách & Phúc lợi thực tập**

Chương trình 12 tuần có cấu trúc tốt (học nền tảng AWS trước, rồi làm dự án). Sự cân bằng giữa học có hướng dẫn (Tuần 1–4) và tự chủ sở hữu dự án (Tuần 5–12) phù hợp với trình độ người tham gia. Có GitHub repo thực, AWS account thực, chi phí AI API thực khiến mọi quyết định đều có ý nghĩa.

---

### Bài học chính

- Khoảnh khắc tốt: Lần đầu tiên toàn bộ AI pipeline — scene detection → Bedrock captioning → pgvector search  chạy end-to-end ở Tuần 7. Nhìn thấy query ngôn ngữ tự nhiên trả về đúng scene video trong thời gian ngắn là khoảnh khắc đột phá.
- Thách thức lớn nhất: Debug Bedrock throttling (Issue #72) dưới áp lực thời gian; giải pháp exponential backoff đòi hỏi hiểu botocore internals lẫn AWS service quotas.
- Có giới thiệu FCAJ không? Chắc chắn. Với sinh viên muốn vượt qua mức học cloud theo tutorial sang sở hữu hệ thống production thực sự, FCAJ là một trong những cơ hội tốt hiện có tại Việt Nam.

---

### Góp ý & Kỳ vọng

- Góp ý 1: Thêm buổi giới thiệu ngắn về AWS cost management best practices từ đầu chương trình nhận thức về chi phí sẽ giúp intern đưa ra quyết định kiến trúc tốt hơn từ sớm.
- Góp ý 2: Cân nhắc tổ chức Demo Day cuối khóa, nơi các dự án intern được trình bày cho AWS engineers hoặc chuyên gia ngành bên ngoài động lực mạnh để đảm bảo chất lượng đầu ra.
- Tương lai: Tôi mong muốn tiếp tục đóng góp cho cộng đồng FCAJ, có thể trong vai trò mentor cho các khóa tiếp theo sau khi tích lũy thêm kinh nghiệm thực tiễn.