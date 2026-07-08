---
title: "Week 9 Worklog"
date: 2026-06-15
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives (15/06 – 21/06/2026):

* Implement the Asset Detail Page UI (per Figma design).
* Build the Upload Video flow with WebSocket real-time progress tracking.
* Migrate AI providers from local Ollama to AWS Bedrock; migrate vector store from ChromaDB to PostgreSQL + pgvector.

### Tasks Carried Out This Week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Mon | **Asset Detail Page:** Build React components — VideoPlayer, TranscriptPanel | 15/06/2026 | 15/06/2026 | Figma design |
| Tue | **Asset Detail Page:** Complete React components — SceneTimeline, InsightsCard (per Figma) | 16/06/2026 | 16/06/2026 | Figma design |
| Wed | **Upload Flow + WebSocket:** Implement multipart upload → S3 presigned URL; WebSocket endpoint for real-time job progress | 17/06/2026 | 17/06/2026 | FastAPI WebSocket docs |
| Thu | Add multi-process support for upload: process multiple files concurrently; state recovery on reconnect | 18/06/2026 | 18/06/2026 | |
| Fri | **APIs & Abstraction:** Build re-ingest/regenerate-insights APIs and refactor AI Provider abstraction layer (swap Ollama ↔ Bedrock) | 19/06/2026 | 19/06/2026 | boto3 docs |
| Sat | **AWS Bedrock & pgvector migration:** Integrate Bedrock Nova Lite/Titan Embeddings; migrate vector storage from ChromaDB to PostgreSQL | 20/06/2026 | 20/06/2026 | pgvector docs |
| Sun | **Architecture adjustment:** Update AWS diagram to reflect Bedrock + pgvector changes; review with mentor | 21/06/2026 | 21/06/2026 | |

### Week 9 Achievements:

* **Asset Detail Page** fully implemented: VideoPlayer with seek-to-scene, TranscriptPanel with timestamps, SceneTimeline, AI Insights panel — matches Figma design closely.
* **Upload + WebSocket:** Users see real-time progress (e.g., "Transcribing… 45%") during pipeline execution; state recovers on browser refresh.
* **Re-Ingest & Regenerate APIs:** Allow users to re-process a video or regenerate AI insights without re-uploading.
* **AI Provider abstraction layer** (`AIProvider` interface): makes switching between local (Ollama) and cloud (Bedrock) providers a config-level decision.
* **AWS Bedrock integrated:** Bedrock Amazon Nova Lite for captioning; Bedrock Titan Embeddings v2 (1024-dim) for semantic embeddings — tested successfully with real keyframes.
* **ChromaDB → pgvector migration complete:** All vector data now stored in PostgreSQL with `vector(1024)` column; cosine similarity queries working via `<=>` operator.
* AWS architecture diagram updated to reflect the final production stack.
