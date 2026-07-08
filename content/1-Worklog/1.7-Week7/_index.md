---
title: "Week 7 Worklog"
date: 2026-06-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives (01/06 – 07/06/2026):

* Complete the full AI pipeline: Scene Detection → Transcription → Captioning.
* Integrate ChromaDB as the vector store for semantic search.
* Expose Search API and Ingest API as async FastAPI endpoints.

### Tasks Carried Out This Week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Mon | Prepare integration checklist for Sprint 2: API endpoints, database migrations, frontend ↔ backend contract verification | 01/06/2026 | 01/06/2026 | |
| Mon | Complete **Scene Detection** module: PySceneDetect + OpenCV, threshold tuning, keyframe extraction to disk | 01/06/2026 | 01/06/2026 | PySceneDetect docs |
| Tue | Complete **Transcription** module: faster-whisper (int8, CPU); segment-level output with timestamps; language detection | 02/06/2026 | 02/06/2026 | faster-whisper GitHub |
| Wed | Complete **Captioning** module: Ollama qwen2.5-vl:3b; resize keyframes to 720p before inference; structured JSON output | 03/06/2026 | 03/06/2026 | Ollama docs |
| Thu | Integrate **ChromaDB** as vector store: store text embeddings (bge-m3, 1024-dim) from captions + transcripts; collection per asset | 04/06/2026 | 04/06/2026 | ChromaDB docs |
| Fri | Build **Ingest API** (`POST /api/v1/ingest`): async FastAPI + BackgroundTasks; pipeline stages run sequentially in background | 05/06/2026 | 05/06/2026 | FastAPI docs |
| Sat | Build **Search API** (`POST /api/v1/search`): embed query → ChromaDB cosine similarity → return ranked scenes with timestamps | 06/06/2026 | 06/06/2026 | |
| Sun | **Benchmark v1:** Run full pipeline on 3 test files; total time 997.55s; identify bottlenecks | 07/06/2026 | 07/06/2026 | |

### Week 7 Achievements:

* **AI pipeline fully operational** end-to-end: upload video → detect scenes → transcribe → caption → embed → store → search.
* Scene Detection: successfully splits a 5-minute video into meaningful scenes using content-aware detection.
* Transcription (faster-whisper int8): accurate Vietnamese speech recognition; segment timestamps preserved.
* Captioning (qwen2.5-vl:3b): generates descriptive scene captions; JSON-structured output with objects, actions, emotions.
* ChromaDB integration: stored 1024-dim bge-m3 embeddings per scene; cosine similarity search working.
* Ingest API returns `202 Accepted` immediately; pipeline runs in background — non-blocking.
* Search API returns semantically relevant scenes for natural language queries (tested with Vietnamese and English queries).
* **Benchmark v1 baseline:** 17s.MOV = 115.31s | 1m58s.MOV = 91.91s | 5m08s.MOV = 790.20s | Total = **997.55s**.
