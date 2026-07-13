---
title: "Self-Assessment"
date: 2026-07-05
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

During my internship at First Cloud AI Journey (FCAJ) from **17/04/2026** to **17/07/2026** as a Cloud/AI Engineer, I completed the project **Building a Smart Media Analytics Platform on AWS**.

The objective of the project was to build a comprehensive video analysis platform combining Generative AI and Serverless/Event-Driven architecture on AWS, supporting Semantic Search. The system is divided into key zones:

* **Frontend & Authentication:** React web application hosted on AWS Amplify with Route 53, user authentication via Amazon Cognito, and API routing through API Gateway and ALB to the ECS Backend API (AWS Fargate).
* **Ingestion & Orchestration:** Video uploads to Amazon S3, triggering event notifications via SQS and EventBridge to AWS Step Functions to coordinate the workflow.
* **AI Processing Pipeline & Persistence:** ECS AI Worker (Fargate) combined with Amazon Transcribe and Amazon Bedrock (Nova Lite & Titan Embeddings) to perform speech-to-text transcription and vector embeddings. Job progress is updated via Redis and WebSockets, and metadata/vectors are persisted in RDS PostgreSQL (pgvector).
* **CI/CD & Observability:** Automated testing and deployment using GitHub Actions (pushing Docker images to ECR), with system monitoring managed by Amazon CloudWatch and AWS X-Ray.

Through this project, I have not only applied my AI/ML knowledge but also designed and deployed a production-grade cloud system conforming to enterprise best practices.

Key achievements include:
* Designed a comprehensive system architecture combining Serverless, Containers, and Generative AI.
* Built an automated video upload and orchestration pipeline using S3, SQS, EventBridge, and Step Functions.
* Implemented the AI processing module (scene detection, speech transcription, vision captioning, vector embedding) running on ECS AI Worker (Fargate).
* Integrated advanced AI services, including Amazon Transcribe and Amazon Bedrock.
* Constructed a real-time status update mechanism utilizing ElastiCache for Redis and WebSocket connections over the Application Load Balancer.
* Stored metadata and executed Vector Search using the pgvector extension on Amazon RDS PostgreSQL.
* Authored the Workshop Website using Hugo, detailing the Proposal, Worklog, Workshop, Blog, Events, Self Evaluation, and Feedback.
* Established an automated CI/CD pipeline using GitHub Actions and deployed monitoring via CloudWatch and AWS X-Ray.
* Performed end-to-end testing and established a resources Cleanup process to optimize AWS usage costs.

Based on my performance, I assess myself against the following criteria:

| No. | Criteria | Rating | Comments |
| --- | --- | --- | --- |
| 1 | Professional knowledge | Good | Successfully applied knowledge in AI/ML, Cloud Computing, FastAPI, and React to build a complete video analysis system. |
| 2 | Ability to learn | Good | Proactively researched and mastered new technologies such as AWS Bedrock, pgvector, Step Functions, and ECS Fargate. |
| 3 | Proactiveness | Good | Actively identified and resolved complex issues including Bedrock throttling, IAM permissions, and VPC networking. |
| 4 | Sense of responsibility | Good | Took full ownership of the AI Pipeline module, ensuring clean code, meeting deadlines, and writing detailed documentation. |
| 5 | Work discipline | Fair | Adhered to group workflows and Git conventions. Need to further refine time management skills for optimal work output. |
| 6 | Communication & presentation | Good | Delivered clear reports with visual architectural diagrams, enabling smooth knowledge sharing and collaboration. |
| 7 | Teamwork & exchange | Fair | Coordinated effectively with frontend engineers on API contracts, but could improve async written documentation on GitHub Issues. |
| 8 | Problem-solving skills | Good | Adopted system-level thinking to divide the pipeline into modular components, facilitating testing and troubleshooting. |
| 9 | Tool usage capability | Good | Proficiently used AWS Console/CLI, Docker, Git, Hugo, and Markdown to support engineering and documentation tasks. |
| 10 | Personal project contribution | Good | Built and delivered the core AI Pipeline and Semantic Search functionalities of the team's Smart Media Analytics product. |
| 11 | Self-evaluation & improvement | Good | Periodically reviewed system architecture and updated documentation based on feedback from FCAJ admins to optimize project quality. |
| 12 | Overall evaluation | Good | Met all internship objectives, establishing a solid understanding of deploying and operating AI applications on cloud infrastructure. |

### Strengths Gained
* Acquired a deep understanding of developing, integrating, and deploying AI/ML systems on AWS cloud infrastructure.
* Proficient in combining Serverless (Lambda, Step Functions, SQS, API Gateway) and Container (ECS Fargate) services to handle heavy workloads.
* Mastered Generative AI integration via Amazon Bedrock (Nova Lite, Titan Embeddings) and vector-based semantic search with pgvector on RDS PostgreSQL.
* Built real-time application updates using Redis Pub/Sub and WebSocket connections.
* Gained hands-on experience with automated CI/CD via GitHub Actions and cloud observability via AWS CloudWatch and AWS X-Ray.

### Areas for Improvement
* Write automated tests (Unit and Integration tests) earlier in the development lifecycle (TDD).
* Further research infrastructure cost-optimization strategies, especially for Amazon Bedrock and continuously running ECS Fargate tasks.
* Increase proactiveness in documenting system design decisions and blockers on public project channels.
* Continue to refine technical writing skills in English for more concise and professional documentation.

### Future Development Plan
* Research and implement MLOps patterns to fully automate retraining, evaluating, and redeploying models when new data becomes available.
* Study Infrastructure as Code (IaC) tools like AWS CDK or Terraform to automate system provisioning.
* Experiment with other LLMs and advanced RAG techniques on Amazon Bedrock to improve video analysis accuracy.
* Enhance English proficiency for better comprehension of official cloud documentation and international collaboration.

### Self-Assessment Conclusion
My internship at FCAJ with the Smart Media Analytics project has been a valuable opportunity to translate theoretical AI and cloud knowledge into a highly functional, real-world application. It has strengthened my Cloud and AI Engineering skills and helped me adopt professional working practices in a project environment.

I assess my internship performance at a **Good** level and feel confident that this experience has built a strong foundation for my future career in Cloud and MLOps engineering.
