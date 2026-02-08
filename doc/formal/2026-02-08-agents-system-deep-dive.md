# 2026-02-08-agents-system-deep-dive

## 1. Agent 核心架構
OpenClaw 的 Agent 系統是整個框架的大腦，負責邏輯推理、上下文管理與工具調用。

### 主要組件：
- **Agent Context (`src/agents/context.ts`)**: 維護當前對話的狀態，包括 User 訊息、Assistant 回覆、Tool Call 與 Tool Result。
- **Model Selection (`src/agents/model-selection.ts`)**: 根據配置、Session 覆寫或指令動態選擇適用的 LLM 模型。
- **Skills & Tools (`src/agents/skills.ts`)**: 管理可用的工具集。工具可以是內建的（如 `read`, `write`, `exec`）或透過 Skill 擴展。
- **Sub-agents (`src/agents/subagent-registry.ts`)**: 支援啟動獨立的子代理 Session 來處理複雜或耗時的任務。

## 2. 工具調用流程 (Tool Use)
1. **Model Output**: LLM 回傳一個包含 `tool_use` 的 JSON。
2. **Parser**: `pi-embedded-subscribe` 解析 Stream 中的工具調用指令。
3. **Registry Check**: 檢查工具是否在允許清單中。
4. **Execution**: 在 Sandbox（Docker）或主機環境執行工具。
5. **Feedback**: 將工具執行結果（Output/Error）封裝後發回給 LLM。

## 3. 記憶體管理 (Memory)
- **Short-term Memory**: 儲存在 Session 的 `.jsonl` 歷史紀錄中。
- **Long-term Memory**: 透過 `memory_search` 與 `memory_get` 工具操作 `MEMORY.md` 與 `memory/` 資料夾下的結構化檔案。

---
*Source: ref/openclaw/src/agents/*
