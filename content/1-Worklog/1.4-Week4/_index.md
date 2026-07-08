---
title: "Week 4 Worklog"
date: 2026-05-11
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives (11/05 – 17/05/2026):

* Understand serverless architecture with API Gateway + Lambda.
* Build and deploy a basic serverless REST API.
* Test APIs using Postman and AWS Console.

### Tasks Carried Out This Week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Mon | Study serverless architecture principles: benefits, limitations, use cases; API Gateway concepts (REST vs. HTTP API) | 11/05/2026 | 11/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| Tue | API Gateway: resources, methods, stages, deployment; integration with Lambda; authorization types (IAM, Cognito, Lambda Authorizer) | 12/05/2026 | 12/05/2026 | https://docs.aws.amazon.com/apigateway/ |
| Wed | Lambda advanced: environment variables, layers, concurrency, error handling, dead-letter queues | 13/05/2026 | 13/05/2026 | https://docs.aws.amazon.com/lambda/ |
| Thu | **Practice:** Build a CRUD REST API with API Gateway + Lambda + DynamoDB (write Lambda code and set up DynamoDB tables) | 14/05/2026 | 14/05/2026 | https://docs.aws.amazon.com/apigateway/ |
| Fri | **Practice:** Configure API Gateway integration, set up stage and deploy the complete REST API | 15/05/2026 | 15/05/2026 | https://docs.aws.amazon.com/apigateway/ |
| Sat | Test API with Postman: GET, POST, PUT, DELETE endpoints; check CloudWatch logs for debugging | 16/05/2026 | 16/05/2026 | |
| Sun | Compare serverless (API Gateway + Lambda) vs. containerized (FastAPI + Docker) — validate the project's App Runner choice | 17/05/2026 | 17/05/2026 | |

### Week 4 Achievements:

* Understood the serverless paradigm and when it is appropriate vs. containerized deployments.
* Built a fully functional CRUD REST API using API Gateway + Lambda + DynamoDB in Python.
* Deployed the API to a "dev" stage and obtained a public invoke URL.
* Successfully tested all CRUD endpoints (GET, POST, PUT, DELETE) using Postman; inspected request/response payloads.
* Debugged Lambda errors using CloudWatch Logs: identified cold start latency (~800ms) and optimized by increasing memory.
* Evaluated trade-offs: for the project's FastAPI backend with WebSocket support and long-running AI processing, AWS App Runner (containerized) is more suitable than Lambda.
