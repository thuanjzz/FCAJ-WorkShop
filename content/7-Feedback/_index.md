---
title: "Sharing and Feedback"
date: 2026-07-05
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

### Overall Evaluation

**1. Working Environment**

The FCAJ working environment is highly collaborative and technically stimulating. Mentors and admin actively facilitate discussions on Slack, provide timely feedback during review sessions, and encourage interns to make real architectural decisions rather than just following instructions. The async-first culture respects individual work styles, which allowed me to deep-focus on complex pipeline problems. If I could suggest one improvement: more in-person (or video call) whiteboarding sessions for architecture discussions would accelerate alignment.

**2. Support from Mentor / Supervisor**

Mentor Nguyen Gia Hung provided guidance at the right level — not giving answers directly, but pointing to the right AWS documentation and asking the right questions to push critical thinking. The feedback sessions in Week 10 where the architecture was reviewed and improved were particularly valuable. I especially appreciated that the mentor trusted me to own the AI pipeline module completely.

**3. Relevance of Work to Academic Major**

The tasks were extremely well-aligned with my Data Science major. I applied concepts from machine learning (embedding models, cosine similarity), software engineering (clean architecture, API design), and cloud computing (AWS managed services) simultaneously. The internship filled a practical gap that university coursework alone cannot provide — particularly in production system design and cloud cost optimization.

**4. Learning & Skill Development Opportunities**

The 11 weeks delivered more hands-on AWS experience than I expected:
- **Weeks 1–4:** Strong AWS fundamentals (EC2, S3, Lambda, DynamoDB, API Gateway, VPC).
- **Weeks 5–11:** Real production AI system development — from Docker Compose prototype to AWS Fargate deployment.

Key new skills gained: AWS Bedrock API, ECS Fargate task configuration, pgvector semantic search, WebSocket real-time architecture, and CI/CD with GitHub Actions → ECR.

**5. Company Culture & Team Spirit**

FCAJ operates like a professional engineering team despite being a training program. Sprint planning, backlog grooming, and code review are taken seriously. The culture of documenting decisions (ADR log, GitHub Issues) and measuring progress quantitatively (benchmark results) directly improved my engineering practices. The team celebrates technical wins and treats failures as learning opportunities — a healthy engineering culture I hope to carry into my career.

**6. Internship Policies & Benefits**

The structured 11-week curriculum (AWS fundamentals first, then project work) is well-designed. The balance between guided learning (Weeks 1–4) and autonomous project ownership (Weeks 5–11) is appropriate for the skill level of participants. Having a real GitHub repository, real AWS accounts, and real AI API costs made every decision feel meaningful.

---

### Key Takeaways

- **Most satisfying moment:** Getting the full AI pipeline — scene detection → Bedrock captioning → pgvector search — working end-to-end for the first time in Week 7. Seeing a natural language query return the exact video scene in under 2 seconds was a breakthrough moment.
- **Biggest challenge:** Debugging AWS Bedrock throttling (Issue #72) under time pressure; the exponential backoff solution required understanding of both botocore internals and AWS service quotas.
- **Would I recommend FCAJ?** Absolutely. For students who want to move beyond tutorial-level cloud learning into production system ownership, FCAJ is one of the best opportunities available in Vietnam.

---

### Suggestions & Expectations

- **Suggestion 1:** Add a brief intro session on AWS cost management best practices at the start — cost awareness would help interns make better architectural decisions earlier.
- **Suggestion 2:** Consider a final demo day where all intern projects are presented to external AWS engineers or industry practitioners — strong motivation for quality output.
- **Future:** I would love to continue contributing to the FCAJ community, potentially as a mentor in future cohorts after gaining more industry experience.
