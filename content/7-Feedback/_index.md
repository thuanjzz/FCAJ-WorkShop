---
title: "Sharing and Feedback"
date: 2026-07-05
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

Throughout the internship and report development in the form of a Workshop Website, I not only had the opportunity to access AWS cloud services but also learned how to systematically present and share a technical project. Compared to traditional report writing, building a website required me to organize the content logically, explain the implementation steps clearly, and ensure that readers can easily follow and reproduce the implementation process.

The project I selected is **Smart Media Analytics** (by team **CloudForge**). This topic helped me connect various acquired knowledge areas, such as data processing, Generative AI, cloud architecture, real-time data streaming, system monitoring, and operational cost optimization.

### 1. Reflections on the Internship Program
What I appreciated most about the internship program at First Cloud AI Journey (FCAJ) was the hands-on project-based learning. Instead of studying individual AWS services in isolation, I was encouraged to combine multiple services to build a comprehensive system for a specific real-world problem.

In the Smart Media Analytics project, each AWS service plays a distinct role:
* **AWS Amplify & Route 53:** Hosts the React frontend and manages domain routing.
* **Amazon Cognito:** Manages user authentication, registration, and security.
* **Amazon S3:** Receives secure video uploads directly from users.
* **Amazon SQS & EventBridge:** Detects upload events and triggers the processing pipeline.
* **AWS Step Functions:** Acts as the orchestrator to coordinate the automated video processing flow.
* **Amazon ECS (AWS Fargate):** Hosts the Backend API and AI Workers for intensive processing.
* **Amazon Bedrock & Amazon Transcribe:** Managed AI services for Speech-to-Text and vector embeddings.
* **ElastiCache for Redis:** Facilitates real-time processing updates via WebSockets.
* **Amazon RDS PostgreSQL (pgvector):** Stores metadata and performs vector-based Semantic Search.
* **Amazon CloudWatch & AWS X-Ray:** Monitors log streams and traces system execution in real-time.

Combining these services gave me a better understanding of building a cohesive Cloud architecture rather than just using services independently.

### 2. Learning and Working Process
During the internship, I had the opportunity to practice self-learning and proactively research official documentation. Many concepts, such as IAM Roles, ECS Task configurations, pgvector semantic search, real-time Generative AI inference, and how Hugo converts Markdown files into a website, were initially very new to me.

Rather than just following instructions blindly, I actively read AWS documentation, experimented with different deployment options, and documented key findings to include in my report.

Throughout the process, I constantly asked myself:
* How is data transmitted between various AWS services?
* What is the exact role of each component within the system?
* Which implementation steps need visual screenshots?
* Which AWS resources should be cleaned up after testing?
* How can someone else easily reproduce this project?

Answering these questions myself helped me understand the system at a deeper level rather than just describing the AWS services used.

### 3. Relevance to Academic Major
This project is highly relevant to my studies in Data Science, Machine Learning, and Cloud Engineering.

Previously, in my machine learning and AI coursework, I focused primarily on model building, training, and accuracy evaluation. However, through this project, I realized that operating a model in production requires a vast support system: data storage, API design, real-time data flow, monitoring, logging, security, and resource management.

This gave me a holistic view of the AI lifecycle when deployed on cloud infrastructure (MLOps/LMOps).

### 4. Results Achieved
What I am most satisfied with is that I built a customized report reflecting my personal contributions rather than just copying a generic workshop template.

I adjusted the report structure to match the Smart Media Analytics project and removed or replaced sections that did not align with my goals.

The report is structured into clean sections:
* Student Information
* Weekly Worklog
* Project Proposal
* Blog Posts
* Events Attended
* Technical Workshop
* Self Evaluation
* Sharing and Feedback

Breaking down the content made it easy to track my progress and ensured a logical and consistent layout.

### 5. Challenges Encountered

#### 5.1. Understanding and Describing the System Architecture
One of the main challenges was grasping the integration of multiple AWS services in a distributed, AI-driven environment.

To make the explanation easier to follow, I divided the architecture into two main parts:
* **AI Processing Pipeline:** Covering ingestion from S3, Step Functions orchestration, speech-to-text (Transcribe), vector embeddings (Bedrock), and storage in RDS PostgreSQL (pgvector).
* **Application API & Search Flow:** Covering the React frontend, routing through API Gateway/ALB, and querying the ECS Backend API for real-time semantic search.

This division made the architecture presentation intuitive and easy to digest.

#### 5.2. Building the Website using Hugo
Learning how Hugo processes Markdown files into a static website was another initial hurdle.

Through hands-on practice, I learned about Hugo’s folder structure, multilingual content management, image organization, and the static directory.

One mistake I encountered was referencing image paths incorrectly due to case sensitivity in folders (e.g., writing `5.13-observability` instead of `5.13-Observability`). After resolving this, I understood how Hugo handles static resources during compilation.

#### 5.3. Selecting Screenshots and Illustrations
Choosing the right screenshots for each step was also challenging. Too many images make the report cluttered, while too few leave the reader confused.

Therefore, I only included screenshots for high-value validation steps:
* Platform architecture diagrams.
* Cognito, Amplify, and Route 53 setup interfaces.
* ECS Task Role configurations and IAM policies.
* The xray-daemon sidecar configuration showing UDP port 2000.
* The AWS X-Ray Console displaying the Service Map.
* CloudWatch log streams showing backend operations.
* The resources cleanup process.

### 6. Suggestions
Based on my experience, I have a few suggestions to enhance the internship program:
* Provide a clear checklist for each report section (Proposal, Worklog, Workshop, Self-evaluation).
* Add detailed guidelines on static file organization and path management in Hugo to prevent link issues.
* Organize a short tutorial on building and testing the Hugo site locally before deployment/submission.
* Encourage students to differentiate clearly between implemented features, theoretical designs, and areas for future study.
* Emphasize the importance of cost optimization and resource cleanup after project completion.

### 7. Overall Evaluation
I would highly recommend this internship program to any student interested in AWS, Cloud Computing, or AI/ML Engineering.

The program not only gives access to modern technologies like Generative AI (Amazon Bedrock) but also provides the opportunity to build a complete project suitable for portfolios or internship reports.

Although it demands a high level of self-learning, the process of researching and solving real-world issues (like Bedrock API throttling) was incredibly rewarding.

### 8. Future Development Plan
Moving forward, I intend to continue developing the Smart Media Analytics platform:
* Implement MLOps/LMOps pipelines to automate embeddings updates when new content is uploaded.
* Automate AWS provisioning using Infrastructure as Code (IaC) tools like Terraform or AWS CDK.
* Test other multimodal LLMs on Amazon Bedrock to speed up video transcription and semantic queries.
* Maintain and synchronize bilingual content between the English and Vietnamese versions.

### 9. Conclusion
This internship clarified how to transform an AI/ML concept into a production-ready Cloud architecture. Beyond technical knowledge, I honed my documentation, presentation, and knowledge-sharing skills through building a technical website.

I learned that the value of a Cloud project lies not in the quantity of AWS services used, but in how they are integrated to solve a practical problem. This experience will serve as a strong foundation for my future growth in Cloud and AI engineering.
