---
tags: [repo/coding/ai, repo/applications, status/planning]
created: 2026-10-22
---
# 🎮 Game-Like AI Team UI (Pixel Office + Discord Command Center)
← Back to [[Home]]

## 🎯 Vision & Core Loop
A 2D top-down "Virtual Studio" (think *Gather.town* or *Stardew Valley* pixel office) where each AI Sub-Agent has an avatar, a desk, and visual state animations. You can assign tasks Either by clicking an agent in the web UI **or** by messaging your centralized **Discord Server**—and watch the agents walk over to each other, hand off data, and execute workflows in real time.

---

## 🏗️ 4-Layer System Architecture (Free-Tier Stack)

```
[ You on Discord ] <---> [ Discord Bot Bridge (Render) ]
                                    │
                                    ▼
[ 2D Game Web UI ] <===> [ Supabase Realtime & DB ] <===> [ Agent Orchestrator ]
  (Vercel + Phaser)      (State, Logs, Task Queue)        (Gemini / Claude / Groq)
```

### 1. 🎨 Frontend: The 2D Game Canvas (Hosted on Vercel)
* **Framework:** Next.js (React) + **Phaser.js** (or **PixiJS** / **Kaboom.js**) for the 2D canvas.
* **UI Overlay:** Tailwind CSS + shadcn/ui for the HUD (token usage counter, task queue sidebar, agent inspector modal).
* **Map & Assets:** Built using **Tiled Map Editor** (free) with 16x16 or 32x32 pixel office sprites (itch.io free assets).
* **Visual Agent States (Driven by WebSockets):**
  * 💤 `IDLE` — Sitting at desk sipping coffee / wandering breakroom.
  * 💭 `THINKING` — Thought bubble animation above sprite head (`...`).
  * 🛠️ `WORKING` — Typing furiously at terminal + tool icon popup (e.g., magnifying glass for web search).
  * 🏃 `HANDOFF` — Agent A physically walks across the office to Agent B's desk to pass a JSON payload.
  * 🚨 `BLOCKED / HUMAN_REVIEW` — Red exclamation mark (`!`) bouncing over head + pings you on Discord.
* **UI/UX Golden Rule Check:**
  * **Empty States:** When no tasks are running, show a cozy "Office is quiet—assign a mission via Discord or Command Bar" banner.
  * **No-Data Placeholders:** Skeleton loaders for agent memory logs and `0 tokens` fallback counters when offline.

### 2. 🔄 State & Event Bus: Real-Time Sync (Supabase)
* **Supabase Realtime (Broadcast & Presence):** Every time an agent changes state or emits a thought, the backend publishes an event to a `studio-events` channel. The Phaser canvas listens to this channel and moves the sprites immediately.
* **PostgreSQL Tables:**
  * `agents` — (`id`, `name`, `role`, `current_state`, `x_coord`, `y_coord`, `model_used`)
  * `missions` — (`id`, `prompt`, `status`, `created_by`, `requires_human_approval`)
  * `agent_logs` — (`id`, `mission_id`, `agent_id`, `thought_summary`, `tool_called`, `tokens_used`, `timestamp`)

### 3. 🧠 Backend: Orchestrator & Sub-Agents (Hosted on Render / Edge Functions)
Built around your Agent Architecture & Guardrail rules:
* **The Manager / Orchestrator Agent:** Receives your high-level prompt, breaks the task into agent-friendly steps, and routes sub-tasks to the right specialist.
* **The Specialist Sub-Agents (Your 4 Core Archetypes + Content/Code):**
  1. 🕵️ **Sales / Niche Research Agent:** Scrapes competitors, social media hooks, or market data.
  2. 📊 **Data Analysis & Cleaning Agent:** Deduplicates, verifies, and formats raw data into clean JSON/Markdown.
  3. 🎧 **Customer Support / Community Agent:** Drafts replies, FAQs, and onboarding flows.
  4. 📋 **HR / Docs Onboarding Agent:** Generates SOPs, summarizes repo documentation, or quizzes you on AWS.
* **Built-In Guardrails & Reliability:**
  * **Not Magic (Strict Schemas):** Force every agent to return structured JSON (`{ "next_state": "HANDOFF", "target_agent": "data_analyst", "speech_bubble": "Cleaned 50 rows! Sending to Analyst.", "payload": {...} }`).
  * **Human Oversight:** If a task is critical (e.g., pushing code or publishing a post), the agent enters `WAITING_APPROVAL` state and pauses until you click "✅ Approve" in Discord or the Web UI.
  * **Error Handling:** Automatic retry with fallback model (e.g., if `gemini-2.5-flash` rate-limits, fall back to Groq `llama-3.3-70b`) and an "Error Sprite" animation if a tool fails.

### 4. 💬 Discord Centralization Bridge (Discord.js / Webhook System)
Instead of building a complex chat UI from scratch, **Discord IS your chat interface**:
* **Multi-Webhook Persona Trick:** Use a single Discord webhook in your `#agent-office` channel. Discord webhooks let you dynamically override the `username` and `avatar_url` on every single message!
  * When the **Research Agent** finishes a step, the webhook posts as **"🕵️ Research Bot"** with its pixel-art avatar.
  * When the **Data Agent** replies, the *same* webhook posts as **"📊 Data Analyst"** with its own avatar.
  * To you, it looks like a live group chat of AI coworkers talking to each other in your Discord server.
* **Two-Way Commands:** Use your main bot (like your `yongbao-bot` setup) to listen for slash commands:
  * `/mission [prompt]` — Starts a new workflow and wakes up the sprites in the web UI.
  * `/approve [mission_id]` — Unblocks an agent waiting for human oversight.
  * `/status` — Returns current token usage and active agent tasks.

---

## 🚀 Phased MVP Build Plan ("Start Simple, Then Expand")

Following the slide rule to **begin with one workflow, then expand**:

- [x] **Phase 1: The Discord "Group Chat" Prototype (Weekend 1)**
  - Create `#agent-office` in your Discord server.
  - Write a simple Node.js/TypeScript script with **1 Orchestrator + 2 Sub-Agents** (Researcher $\rightarrow$ Writer) that talk to each other in Discord using dynamic webhook avatars.
- [x] **Phase 2: Supabase State + Static 2D Room (Weekend 2)**
  - Connect the script to Supabase so every message updates an `agents` status table.
  - Spin up a basic Next.js + Phaser canvas with 3 pixel desks and sprites that change speech bubbles when Supabase updates.
- [x] **Phase 3: Walking Animations & Human-in-the-Loop (Weekend 3)**
  - Add A* pathfinding (`easystarjs`) so Agent A walks to Agent B's desk during a `HANDOFF` event.
  - Add Discord button interactions (`Approve` / `Reject`) for critical tasks.
- [ ] Phase 4: Coding Team & Video Editing Team