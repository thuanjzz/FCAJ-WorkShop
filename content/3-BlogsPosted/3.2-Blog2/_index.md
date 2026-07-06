---
title: "Blog 2"
date: 2026-07-05
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# AWS Bedrock Throttling: Exponential Backoff Strategy for Production AI Systems

### 1. The Throttling Challenge in Serverless AI

When scaling AI pipelines that invoke Foundation Models (FMs), API rate limits are a common bottleneck. In our Video Semantic Search project (`smart_media_analytics_cloudforge`), the pipeline extracts keyframes from a video and sends them to **AWS Bedrock (Amazon Nova Lite)** to generate descriptive captions. 

With large batches of keyframes processed concurrently, we immediately encountered a `ThrottlingException` (HTTP 429). By default, AWS Bedrock imposes a limit of 100 requests per minute. Without proper handling, these transient errors crash the pipeline containers, resulting in failed jobs.

---

### 2. Understanding Exponential Backoff and Jitter

To build a resilient system, we must handle API rate limits gracefully. Instead of failing immediately or retrying continuously (which worsens the load on the API), we implement **Exponential Backoff with Jitter**.

- **Exponential Backoff:** The wait time between retries increases exponentially (e.g., 1s, 2s, 4s, 8s).
- **Jitter (Random Noise):** Adds a random delay to prevent "thundering herd" problems where all failed instances retry at the exact same millisecond.

\[T_{\text{wait}} = 2^{\text{attempt}} \times \text{base\_delay} + \text{random\_jitter}\]

---

### 3. Implementing the Solution in Python

We solved this issue using two layers of defense: `botocore`'s built-in adaptive config and the python `tenacity` library for custom control.

#### Layer 1: Botocore Adaptive Retry Config
We configured the `boto3` client to use the `adaptive` retry mode, which automatically throttles local requests based on feedback from Bedrock.

```python
import boto3
from botocore.config import Config

# Configure adaptive retry with 5 max attempts
config = Config(
    retries={
        'max_attempts': 5,
        'mode': 'adaptive'
    }
)

bedrock_client = boto3.client('bedrock-runtime', config=config)
```

#### Layer 2: Tenacity Retry Decorator
For precise control over specific API calls, we wrap our Bedrock invocation function with the `tenacity` retry decorator:

```python
from tenacity import retry, stop_after_attempt, wait_random_exponential
from botocore.exceptions import ClientError

@retry(
    stop=stop_after_attempt(5),
    wait=wait_random_exponential(multiplier=1, max=10),
    retry=lambda e: isinstance(e, ClientError) and e.response['Error']['Code'] == 'ThrottlingException'
)
def invoke_bedrock_model(client, prompt, image_bytes):
    # API invocation logic here
    pass
```

---

### 4. Verification with Botocore Stubber

To verify that our retry logic handles `ThrottlingException` correctly without waiting for real API limits, we write unit tests using `botocore.stub.Stubber` to mock API responses:

```python
from botocore.stub import Stubber

def test_bedrock_throttling_retry():
    client = boto3.client('bedrock-runtime')
    stubber = Stubber(client)
    
    # Mock a ThrottlingException on the first call, and success on the second
    stubber.add_client_error('invoke_model', service_error_code='ThrottlingException', http_status_code=429)
    stubber.add_response('invoke_model', service_response={})
    
    with stubber:
        # Call the wrapped invoke function
        response = invoke_bedrock_model_with_retry(client, "test prompt", b"")
        assert response is not None
```

---

### 5. Conclusion

API rate limits are a reality in production cloud environments. By integrating an adaptive retry strategy with exponential backoff and jitter, our FastAPI backend handles temporary Bedrock rate limits seamlessly, ensuring that large video files process successfully without user intervention.