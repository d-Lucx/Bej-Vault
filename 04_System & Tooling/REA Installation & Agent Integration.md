---
title: "REA Platform & Reverse-Engineering Agent Integration"
created: 2026-10-28
updated: 2026-10-28
tags:
  - system/tooling
  - agent/rea
  - reverse-engineering
  - mcp
  - codex
  - portfolio-audit
aliases:
  - REA Integration
  - Agent REA
  - worker-rea
---

← Back to [[Home]]

# 🔬 REA Installation & Agent Integration

> [!success] Integration Status: Fully Operational & Aligned
> The **REA (Reverse Engineer Anything)** platform and Model Context Protocol (MCP) server have been installed, verified, and connected to agent **`worker-rea`** using Codex GPT (`gpt-6.1-sol`) on the Hive office floor.

---

## 1. Connection & Health Verification

Diagnostics executed via `npx rea-agents doctor` and system registries confirm full operational alignment:

| Component | Status | Details |
| :--- | :---: | :--- |
| **Codex MCP Client** | `aligned` | Registered under `[mcp_servers.rea]` in `~/.codex/config.toml` |
| **Config Backup** | `Preserved` | Saved to `config.toml.rea.backup` |
| **MCP Execution Target** | `Active` | `cmd.exe /d /c npx -y rea-agents@6.8.0 mcp` |
| **REA Skill Version** | `v49 aligned` | Installed at `~/.agents/skills/reverse-engineer-anything` |
| **Catalog Scope** | `Ready` | **229 MCP tools**, **175 CLI commands**, **6 MCP prompts** |
| **Hive Agent `worker-rea`** | `Active / On-Demand` | ID: `worker-rea` \| Provider: `codex (gpt-6.1-sol)` \| Breaker: `healthy` |
| **Hire Manifest** | `Created` | `hive/research/hires/rea.agent.json` |
| **Task Ledger** | `Done` | Logged as ticket `YPR-37` in `tasks.json` and `board.md` |

---

## 2. Available Tool Suite (229 MCP Tools & 175 CLI Commands)

REA equips the office with evidence-backed inspection tools grouped across five major reverse-engineering domains:

### A. JavaScript, Electron & Web Application Tools
- `analyze-javascript-application` / `analyze_javascript_application`: Statically reconstructs local JavaScript/Electron applications and ASAR bundles into explicit dependency graphs.
- `inspect_electron_fuses`: Inspects runtime fuses in Electron binaries (e.g., checking if Node CLI inspect arguments are enabled or locked).
- `analyze-web-bundle`: Captures and inspects front-end web bundle modules and sourcemaps.
- `recover-javascript-sources`: Recovers readable JavaScript modules with verified provenance.
- `reconcile-javascript-runtime`: Correlates static JavaScript application graphs with live runtime traces.

### B. Native Binary & Disassembly / Decompilation
- `open_binary`: Opens native Mach-O, PE/COFF, or ELF binaries to establish authenticated static evidence.
- `decompile` / `decompile-all`: Emits decompiled C/pseudo-code for target functions, procedures, or file offsets.
- `search-bytes` / `read-bytes`: Finds byte patterns, string constants, and raw memory slices.
- `xrefs`: Lists bounded cross-references to addresses and routines.
- `resolve-native-call-targets`: Maps call-sites and dynamic dispatch tables.
- `debug-with-rizin` / `debug-with-gdb`: Ephemeral low-level debugger interface sessions.

### C. Mobile (Android APK & Apple IPA)
- `inspect_android_package`: Ingests APK packages to extract manifests, activities, and services.
- `decode-android-resources`: Decodes resources and binary XML layouts.
- `search-android-classes` & `trace-android-references`: Indexes and traces Android DEX classes and cross-method invocations.
- `project-apple-application-graph`: Analyzes iOS IPA and macOS application bundles and plists.

### D. Dynamic Runtime Observation & Network Captures
- `capture-electron-scenario`: Runs deterministic, cancellable scenarios on Electron applications.
- `capture-browser-scenario`: Executes automated browser scenarios via Playwright to capture dynamic evidence.
- `inspect_web_network_capture`: Decodes HAR, SAZ (Fiddler), and mitmproxy traces without leaking secrets.
- `list_browser_targets` / `list_electron_targets`: Connects to active live processes for inspection.

### E. Ledger & Differential Analysis
- `build-reconstruction-obligation-ledger`: Synthesizes an evidence-backed ledger detailing confirmed structural facts vs. remaining unknowns.
- `compare-source-to-bundle`: Compares source repositories against compiled/packaged deployment artifacts.
- `compare-application-versions`: Computes semantic diffs across application versions.

---

## 3. Core Operational Workflows

Agent **REA** operates under five primary standardized workflows:

1. **Electron & Desktop App Reconstruction Workflow**:
   - Ingests the target `.asar` or packaged application directory.
   - Extracts internal webpack/vite module graphs, IPC channels (`ipcRenderer` $\leftrightarrow$ `ipcMain`), preload bridges, and exposed APIs.
   - Flags security boundaries and disabled fuses.
2. **Evidence-Backed Obligation Ledger Workflow**:
   - Eliminates guesswork by enforcing structural evidence before reaching conclusions.
   - Produces a deterministic ledger separating verified facts (digests, offsets, explicit AST shapes) from unresolved unknowns.
3. **Target Routing & Static Binary Inspection Workflow**:
   - Identifies binary architecture (`x86_64`, `ARM64`, `.NET NativeAOT`, `PE/ELF`).
   - Resolves procedure graphs, identifies entrypoints, and decompiles critical routines.
4. **Network & Web Application Protocol Reconstruction**:
   - Ingests HAR/SAZ recordings or live browser scenarios to reverse-engineer undocumented REST, GraphQL, or WebSocket protocols.
   - Projects serverless AgentEnv bundles for offline replay.
5. **Differential Version & Patch Analysis**:
   - Performs side-by-side comparison between two build artifacts or between historical source and production bundles to detect behavioral mutations or stealth updates.

> [!info] Floor Readiness & Reaper Protocol
> Agent `worker-rea` is registered on the floor. While idle workers are automatically reaped after 20 minutes to conserve tokens, the underlying MCP registration, skills, and hire manifest remain permanent. Any reverse-engineering dispatch to `worker-rea` spins up a session instantly.

---
