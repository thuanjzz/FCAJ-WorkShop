---
title: "Week 5 Worklog"
date: 2026-05-18
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives (18/05 – 24/05/2026):

* Kick off **Sprint 1** of the `smart_media_analytics_cloudforge` project.
* Define API Data Contract and database schema between frontend and backend.
* Set up Docker Compose development environment: frontend, backend, ChromaDB.
* Build the basic AI pipeline and a working FastAPI backend.

### Tasks Carried Out This Week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Mon | Sprint 1 planning: define user stories, task breakdown, set up GitHub Project board | 18/05/2026 | 18/05/2026 | GitHub Issues Board |
| Tue | Design API Data Contract: OpenAPI spec for Ingest API, Search API; define request/response schemas | 19/05/2026 | 19/05/2026 | |
| Wed | Design PostgreSQL schema: `assets`, `scenes`, `transcripts`, `embeddings` tables; vector column with pgvector | 20/05/2026 | 20/05/2026 | |
| Thu | Set up Docker Compose: FastAPI + Uvicorn, PostgreSQL 16, ChromaDB, Redis; configure environment variables | 21/05/2026 | 21/05/2026 | |
| Fri | Build basic FastAPI backend: health check endpoint, file upload endpoint (multipart), PostgreSQL connection via SQLAlchemy | 22/05/2026 | 22/05/2026 | |
| Sat | Build basic AI pipeline (local): FFmpeg scene detection → faster-whisper transcription → Ollama captioning flow | 23/05/2026 | 23/05/2026 | |
| Sun | Initialize React 19 + Vite + Tailwind CSS frontend; basic routing (Upload page, Dashboard page); connect to backend API | 24/05/2026 | 24/05/2026 | |

### Week 5 Achievements:

* Completed Sprint 1 planning with 15+ user stories in GitHub Issues; set up Agile workflow with labels and milestones.
* Defined the API Data Contract (OpenAPI 3.0 spec) — clear interface agreement between frontend and backend teams.
* Designed the PostgreSQL schema: `assets`, `scenes`, `transcripts`, `tags` tables with proper indexes and foreign keys.
* Docker Compose environment running successfully: FastAPI (:8000), PostgreSQL (:5432), ChromaDB (:8001), Redis (:6379).
* FastAPI backend serving: `POST /api/v1/ingest` (file upload), `GET /api/v1/health`, `GET /api/v1/assets`.
* Basic AI pipeline prototype: FFmpeg extracts keyframes → faster-whisper transcribes audio → Ollama (qwen2.5-vl:3b) generates captions.
* React frontend initialised with Vite; Tailwind CSS configured; basic Upload and Dashboard pages routing working.
