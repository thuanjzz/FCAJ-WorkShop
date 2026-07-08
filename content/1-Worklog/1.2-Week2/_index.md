---
title: "Week 2 Worklog"
date: 2026-04-27
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives (27/04 – 03/05/2026):

* Explore and practice with core AWS services: S3, Lambda, VPC.
* Understand AWS networking fundamentals and cloud architecture patterns.
* Complete hands-on labs for each service.

### Tasks Carried Out This Week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Mon | Study Amazon S3: bucket creation, object storage, versioning, lifecycle policies, presigned URLs, static website hosting | 27/04/2026 | 27/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| Tue | **Practice S3:** Create bucket, upload/download objects, enable versioning, configure lifecycle rules, host a static website | 28/04/2026 | 28/04/2026 | https://docs.aws.amazon.com/s3/ |
| Wed | Study AWS Lambda: event-driven architecture, triggers (S3, API Gateway, SQS), execution environment, cold start, IAM roles | 29/04/2026 | 29/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| Thu | **Practice Lambda:** Create a Python Lambda function triggered by S3 events; test with sample events | 30/04/2026 | 30/04/2026 | https://docs.aws.amazon.com/lambda/ |
| Fri | Study VPC: subnets (public/private), route tables, Internet Gateway, NAT Gateway, Security Groups vs NACLs | 01/05/2026 | 02/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| Sun | **Practice VPC:** Create a custom VPC with public/private subnets; deploy EC2 in each; test connectivity | 03/05/2026 | 03/05/2026 | https://docs.aws.amazon.com/vpc/ |

### Week 2 Achievements:

* Mastered Amazon S3: bucket policies, versioning, lifecycle rules, static website hosting, and presigned URLs.
* Created a Lambda function in Python that reacts to S3 upload events and logs metadata to CloudWatch.
* Understood VPC architecture: CIDR blocks, public vs. private subnets, route tables, Internet/NAT Gateway, and Security Groups.
* Successfully built a custom VPC with 2 AZs, deployed EC2 instances in both public and private subnets, and verified routing.
* Gained understanding of how these services interconnect as the foundation for cloud-native architectures.
