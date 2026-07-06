---
title: "Event 1"
date: 2026-04-17
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# First Cloud AI Journey — Cohort Kickoff & AWS Orientation

### 1. Purpose of the Event

The **Cohort Kickoff & AWS Orientation** marked the official launch of the First Cloud AI Journey (FCAJ) internship program. The event aimed to:
- Connect interns with corporate mentors and peer collaborators.
- Set expectations regarding code quality, collaboration rules, and the Agile (Scrum) sprint lifecycle.
- Provide a deep dive into the business goals and technical scope of the target project: **Smart Media Analytics CloudForge** (Video Semantic Search).
- Guide interns through setting up their IAM credentials, AWS CLI configuration, and access to AWS Management Console.

---

### 2. Speakers & Mentors

- **Nguyen Gia Hung** — Senior Cloud Solutions Architect & Company Mentor (hunggia@amazon.com)
- **FCAJ Admin Team** — Program coordinators and agile coaches.

---

### 3. Key Highlights & Activities

#### Agile & Scrum Setup
- The team established a 2-week sprint cycle.
- Introduced GitHub Issues Board for tracking task progression, PR reviews, and issue logging (e.g., configuring branch naming policies).
- Clarified the definition of done (DoD) for pull requests: clean code, pass local linter, and successful Docker build.

#### Project Scope Briefing
- **The Mission:** Build an AI-powered Video Semantic Search engine.
- **Role Alignment:** Pham Ninh Thuan was assigned ownership of the core **AI Pipeline module** (Scene detection, ASR transcription, Bedrock image captioning, and vector persistence).
- **Architecture Draft:** Discussed the local (Docker) ↔ cloud (AWS App Runner, RDS pgvector, Bedrock, S3) mapping architecture.

#### AWS Hands-On Orientation
- Guided walk-through on creating IAM users with the least privilege principle.
- Standardized AWS CLI setup: configuring Access Key, Secret Key, default region (`us-east-1`), and enforcing MFA for root accounts.

---

### 4. Key Learnings & Takeaways

- **Ownership Mentality:** As the owner of the AI pipeline, I realized the need to document API contracts early so that the frontend and database tasks wouldn't be blocked.
- **Least Privilege Principle:** Configured IAM policies with narrow permission scopes (denying wildcard `*` actions where possible) to prevent security slip-ups in cloud environments.
- **Agile Discipline:** Mastered task estimation and learned to break complex architectural problems (like batch image processing) into smaller, trackable GitHub issues.

---

### 5. Application to Project Work

Immediately after the event, I successfully initialized my AWS CLI environment, created the local Docker Compose configuration, and drafted the first OpenAPI Data Contract, setting a solid foundation for Sprint 1.
