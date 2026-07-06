---
title: "Blog 2"
date: 2026-07-05
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# AWS Bedrock Throttling: Chiến lược Exponential Backoff cho Hệ thống AI Production

### 1. Thách thức Throttling trong hệ thống AI Serverless

Khi vận hành các pipeline AI tích hợp các mô hình ngôn ngữ lớn (LLMs) ở quy mô thực tế, giới hạn tần suất gọi API (Rate Limits) là một rào cản rất lớn. Trong dự án Video Semantic Search (`smart_media_analytics_cloudforge`), pipeline của chúng tôi trích xuất các keyframe từ video và gửi chúng đến **AWS Bedrock (Amazon Nova Lite)** để tạo mô tả tự động (captioning).

Khi xử lý đồng thời một lượng lớn keyframe, chúng tôi ngay lập tức gặp lỗi `ThrottlingException` (HTTP 429). Mặc định, AWS Bedrock giới hạn 100 requests mỗi phút. Nếu không được xử lý đúng cách, lỗi tạm thời này sẽ làm crash các container xử lý, dẫn đến hỏng job của người dùng.

---

### 2. Hiểu về Exponential Backoff và Jitter

Để xây dựng một hệ thống hoạt động ổn định, chúng ta cần xử lý các lỗi vượt giới hạn tần suất gọi API một cách mềm dẻo. Thay vì báo lỗi ngay lập tức hoặc gửi yêu cầu liên tục (làm trầm trọng thêm tải của hệ thống), ta áp dụng chiến lược **Exponential Backoff kết hợp Jitter**.

- **Exponential Backoff:** Thời gian chờ giữa các lần retry tăng dần theo cấp số nhân (ví dụ: 1 giây, 2 giây, 4 giây, 8 giây).
- **Jitter (Độ trễ ngẫu nhiên):** Thêm một khoảng thời gian trễ ngẫu nhiên vào mỗi lần thử lại để ngăn hiện tượng "thundering herd" (khi tất cả các tiến trình bị lỗi đồng loạt thử lại tại cùng một mili-giây).

\[T_{\text{wait}} = 2^{\text{attempt}} \times \text{base\_delay} + \text{random\_jitter}\]

---

### 3. Triển khai giải pháp trên Python

Chúng tôi giải quyết vấn đề này qua hai lớp phòng thủ: sử dụng cơ chế retry adaptive tích hợp sẵn của `botocore` và thư viện Python `tenacity` để tùy biến sâu.

#### Lớp 1: Cấu hình Adaptive Retry của Botocore
Chúng tôi cấu hình cho client `boto3` sử dụng chế độ retry `adaptive`. Chế độ này tự động điều chỉnh tốc độ gửi request dựa trên phản hồi từ phía Bedrock.

```python
import boto3
from botocore.config import Config

# Cấu hình adaptive retry tối đa 5 lần thử lại
config = Config(
    retries={
        'max_attempts': 5,
        'mode': 'adaptive'
    }
)

bedrock_client = boto3.client('bedrock-runtime', config=config)
```

#### Lớp 2: Sử dụng Decorator của thư viện Tenacity
Để kiểm soát chính xác hơn đối với các hàm gọi API cụ thể, chúng tôi bọc hàm gọi Bedrock bằng decorator retry của `tenacity`:

```python
from tenacity import retry, stop_after_attempt, wait_random_exponential
from botocore.exceptions import ClientError

@retry(
    stop=stop_after_attempt(5),
    wait=wait_random_exponential(multiplier=1, max=10),
    retry=lambda e: isinstance(e, ClientError) and e.response['Error']['Code'] == 'ThrottlingException'
)
def invoke_bedrock_model(client, prompt, image_bytes):
    # Logic gọi API Bedrock tại đây
    pass
```

---

### 4. Kiểm thử và xác minh bằng Botocore Stubber

Để đảm bảo logic retry hoạt động chính xác trước lỗi `ThrottlingException` mà không cần chờ đến khi chạm hạn mức thực tế, chúng tôi viết unit test sử dụng `botocore.stub.Stubber` để giả lập (mock) phản hồi API:

```python
from botocore.stub import Stubber

def test_bedrock_throttling_retry():
    client = boto3.client('bedrock-runtime')
    stubber = Stubber(client)
    
    # Mock lỗi ThrottlingException ở lần đầu và thành công ở lần gọi thứ hai
    stubber.add_client_error('invoke_model', service_error_code='ThrottlingException', http_status_code=429)
    stubber.add_response('invoke_model', service_response={})
    
    with stubber:
        # Gọi hàm được bọc logic retry
        response = invoke_bedrock_model_with_retry(client, "test prompt", b"")
        assert response is not None
```

---

### 5. Kết luận

Giới hạn rate limit là thực tế hiển nhiên khi vận hành hệ thống cloud production. Bằng cách tích hợp chiến lược retry adaptive kết hợp exponential backoff và jitter, backend FastAPI của chúng tôi xử lý mượt mà các lỗi 429 tạm thời, đảm bảo video dài được xử lý hoàn tất mà không cần người dùng thao tác lại.