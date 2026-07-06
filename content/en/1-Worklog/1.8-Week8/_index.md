---
title: "Week 8 Worklog"
date: 2026-06-08
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives (08/06 – 14/06/2026):

* Optimize the AI pipeline: switch ASR model, update embedding model.
* Design Dashboard UI components based on Figma.
* Unify AWS cloud architecture and draw the system diagram.

### Tasks Carried Out This Week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Mon | **ASR Migration:** Switch from WhisperX to OpenAI Whisper (base) for better stability; update pipeline integration | 08/06/2026 | 08/06/2026 | ADR log |
| Tue | **Benchmark v2:** Re-run pipeline after ASR change; 17s.MOV improved from 115.31s → **57.45s** (~50% faster) | 09/06/2026 | 09/06/2026 | |
| Wed | Update Embedding model configuration; validate bge-m3 1024-dim output quality with sample Vietnamese text | 10/06/2026 | 10/06/2026 | |
| Thu | Design **Dashboard UI** components in Figma: Video Card, Status Badge, Search Bar, Tag Filter, Pagination | 11/06/2026 | 11/06/2026 | Figma: Visualization-Of-SMA |
| Fri | Design **Upload Page** UI in Figma: drag-and-drop zone, progress bar, file metadata preview | 12/06/2026 | 12/06/2026 | Figma: Visualization-Of-SMA |
| Sat | Design **Login / Register** and **Asset Detail** page wireframes in Figma | 13/06/2026 | 13/06/2026 | Figma: Visualization-Of-SMA |
| Sun | Unify AWS cloud architecture: finalize service mapping (Local ↔ AWS); draw architecture diagram using draw.io | 14/06/2026 | 14/06/2026 | |

### Week 8 Achievements:

* **ASR stabilized:** Switched to OpenAI Whisper (base) — better compatibility with the pipeline, no dependency conflicts.
* **Pipeline v2 benchmark:** 17s.MOV processing time halved (115.31s → 57.45s); total for 3 files: **972.09s** (−2.5% vs v1).
* All 4 Figma screens completed: Dashboard, Upload, Login/Register, Asset Detail — consistent design system with dark theme.
* AWS architecture diagram finalized: 14-step data flow from user access to vector search result, covering all AWS services.
* Architecture decision confirmed: Local (Docker) ↔ AWS mapping for all 10 components documented in ADR table.
