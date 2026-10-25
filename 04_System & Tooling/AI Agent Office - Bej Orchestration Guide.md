---
title: "AI Agent Office Guide: Bej (Orchestrator) & Autonomous Teams"
created: 2026-10-25
updated: 2026-10-25
tags:
  - ai-agents
  - orchestration
  - munder-difflin
  - workflows
  - prompts
aliases:
  - Agent Office Guide
  - Bej Orchestrator Playbook
---

# 🤖 AI Agent Office — Orchestration & Usage Guide

> [!abstract] Overview
> This guide outlines how to utilize **Bej (god)** and the autonomous multi-agent office for building applications, conducting research, producing marketing assets, and automating programmatic media workflows.

---

## 🏛️ 1. Architecture: How the Office Works

The workspace operates as an autonomous collaborative office:

```
                  ┌──────────────────────────────┐
                  │      Human (You)             │
                  └──────────────┬───────────────┘
                                 │ Prompts / Goals / Approvals
                                 ▼
                  ┌──────────────────────────────┐
                  │      Bej (god)               │
                  │   Autonomous Orchestrator    │
                  └──────┬───────────────┬───────┘
                         │               │
        Decomposes & Plans               Dispatches Contracts
                         │               │
          ┌──────────────▼───┐       ┌───▼──────────────────────────┐
          │ Shared Hive      │       │ Ephemeral Workers ("Temps")  │
          │ • board.md       │       │ • Worker A (Frontend/UI)     │
          │ • tasks.json     │       │ • Worker B (Backend/API)     │
          │ • MemPalace      │       │ • Worker C (Research/Data)   │
          └──────────────────┘       └──────────────────────────────┘
```

### Core Roles
- **Bej (god)**: The executive orchestrator. Handles decomposition, milestones, task assignments, board updates, worker dispatch, git branch integration, and QA.
- **Workers ("Temps")**: Ephemeral sub-agents spun up for discrete, single-focus jobs. Each temp runs in an isolated git worktree with its own token cap and reports back upon completion.
- **Engines Available**:
  - `claude` (Claude Code CLI)
  - `codex` (OpenAI Codex CLI)
  - `antigravity` (Native DeepMind agentic harness)
- **Semantic Memory**: Powered by `mempalace` (AAAK dialect / vector store) to recall institutional knowledge and project decisions across sessions.

---

## 📋 2. The Golden Rule: The 4-Part Dispatch Contract

To get predictable, production-grade results without back-and-forth babysitting, structure prompts with these 4 dimensions:

| Dimension | Purpose | Example |
| :--- | :--- | :--- |
| **OBJECTIVE** | High-level goal and context | *"Build an invoice management REST API"* |
| **OUTPUT** | Concrete deliverable and file path | *"Fastify TypeScript server in `apps/api/` with unit tests"* |
| **TOOLS & ENGINES** | Libraries, CLIs, or worker assignments | *"Use `codex` or `claude`; use SQLite and Zod"* |
| **BOUNDARIES** | Scope limits, constraints, and DoD | *"No external cloud services; must pass all test suites"* |

---

## 🚀 3. Domain Playbooks & Prompt Templates

### 💻 A. Apps, Websites & Webapps

> [!tip] Best Practice
> Ask Bej to spin up parallel workers for frontend and backend in isolated worktrees, then review and merge them.

#### Prompt Example: Full-Stack Kanban Application
```markdown
OBJECTIVE:
Build a modern Kanban web application for remote teams.

OUTPUT:
A fully runnable local repository in `C:\Yan2_Projects\kanban-app` with:
1. Backend: SQLite database, Prisma ORM, and Fastify REST API.
2. Frontend: Next.js (App Router), Tailwind CSS, Lucide icons, and drag-and-drop board support.
3. Automated test suite and a README with quickstart commands.

ORCHESTRATION & TOOLS:
- Decompose this into backend and frontend tasks in tasks.json.
- Spin up two temps to implement the backend API and frontend components in parallel.
- Verify integration and ensure `npm test` and `npm run build` pass without errors.

BOUNDARIES:
- Keep authentication local (session/JWT tokens, no external SaaS dependencies).
- Ensure zero TypeScript compiler errors.
```

---

### 🔬 B. Deep Research & Technical Analysis

> [!tip] Best Practice
> Leverage search and web capabilities, then instruct Bej to index key findings into MemPalace for cross-session recall.

#### Prompt Example: Vector Database Benchmarking
```markdown
OBJECTIVE:
Perform a comprehensive technical evaluation of open-source vector databases (pgvector, ChromaDB, Qdrant, Milvus).

OUTPUT:
A structured markdown report saved to `research/vector-db-benchmark.md` featuring:
1. Quantitative trade-offs (memory overhead, indexing latency at 1M vectors).
2. Operational ease (self-hosting complexity, Python & Node.js SDK maturity).
3. Recommendation matrix for an offline-first autonomous agent architecture.

BOUNDARIES:
- Focus on real-world edge/desktop constraints rather than multi-node enterprise clusters.
- Mine the final durable findings into MemPalace once approved.
```

---

### 📣 C. Marketing & Content Creation

> [!tip] Best Practice
> Request multi-format asset packs (blog, Twitter/X thread, landing page copy, email sequence) in a single unified campaign.

#### Prompt Example: Open-Source Product Launch
```markdown
OBJECTIVE:
Create a full Go-To-Market (GTM) launch content pack for our open-source AI tooling harness.

OUTPUT:
Create the following files under `content/launch-v1/`:
1. `announcement.md`: Technical launch blog post formatted for Hacker News (Show HN) and Dev.to.
2. `social-thread.md`: 5-part educational Twitter/X thread breaking down the multi-agent architecture.
3. `landing-page.md`: Hero headline, subheadline, 3 core value pillars, feature breakdown, objection handling, and FAQ.
4. `changelog.md`: User-centric release notes highlighting key benefits.

BOUNDARIES:
- Avoid buzzword-heavy marketing fluff; maintain a clear, developer-first technical voice.
```

---

### 🎬 D. Video Editing & Programmatic Media

> [!tip] Best Practice
> Use script-driven CLI pipelines (FFmpeg, Whisper, Remotion, MoviePy) to batch-process media without manual video editor software.

#### Prompt Example: Automated Short-Form Video Pipeline
```markdown
OBJECTIVE:
Build an automated batch processing pipeline for video clips in `C:\Yan2_Projects\media\raw`.

OUTPUT:
1. A PowerShell script `scripts/make-shorts.ps1` utilizing FFmpeg to:
   - Convert 1920x1080 landscape video to 1080x1920 vertical format (centered with blurred background margins).
   - Normalize audio to -14 LUFS.
2. Subtitle generation pipeline using Whisper CLI to create and burn `.srt` captions into the video.
3. A test run on one sample clip with the processed output saved to `media/export/`.

BOUNDARIES:
- Rely strictly on installed CLI utilities (FFmpeg, Python). Do not invoke interactive GUI software.
```

---

## ⚡ 4. Operational Commands & Shortcuts

### Interacting with the Orchestrator
- **View Plan**: Read [board.md](file:///C:/Yan2_Projects/hive/board.md) for the live high-level plan.
- **View Tasks**: Read [tasks.json](file:///C:/Yan2_Projects/hive/tasks.json) for the kanban status (`todo`, `doing`, `blocked`, `done`).
- **Human Escalations (ASK ME)**: If a card requires credentials, payment, or executive decision, Bej sets it to `blocked` and attaches a short question in `humanQA`. Reply directly in chat or via the ASK ME tab.
- **Spawning Temps**: Say `"spin up a worker for X"` or `"get two temps on this"`.
- **Compaction**: When context grows long, run `/compact` to summarize history while preserving durable memory.

---

> [!note] Reference Paths
> - **Shared Hive**: `C:\Yan2_Projects\hive`
> - **Tasks Kanban**: `C:\Yan2_Projects\hive\tasks.json`
> - **Live Board**: `C:\Yan2_Projects\hive\board.md`
> - **God Workspace**: `C:\Yan2_Projects\hive\agents\god`
