# 2026-02-08-session-management-deep-dive

## 1. Session 的定義
在 OpenClaw 中，Session 是與特定使用者或群組對話的持久化實體。它封裝了對話歷史、Agent 配置、自定義模型設定以及當前狀態。

## 2. 存儲機制 (Persistence)
- **JSONL Format**: 每個 Session 的訊息紀錄都存儲在 `.jsonl` 檔案中，這便於流式讀取與追加 (Append)。
- **Metadata**: 包括 `sessionId`, `sessionKey`, `agentId`, `modelOverride` 等。
- **Store Path**: 通常位於工作目錄下的 `sessions/` 或由 `session.storePath` 配置。

## 3. Session 狀態與過濾
- **Transcript**: 包含 `user`, `assistant`, `system`, `tool_call`, `tool_result` 等角色。
- **Compaction**: 系統會定期對長對話進行壓縮（Compaction），以節省 Token 並保持上下文整潔。
- **Context Guard**: 防止對話長度超過模型限制，必要時進行裁減。

## 4. 關鍵檔案
- `src/gateway/session-utils.ts`: 提供讀寫 Session 檔案的工具函式。
- `src/agents/compaction.ts`: 實現對話壓縮邏輯。
- `src/sessions/send-policy.ts`: 管理訊息發送策略（如頻率限制）。

---
*Source: ref/openclaw/src/*
