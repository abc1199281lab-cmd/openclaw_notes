# 2026-02-08-cron-heartbeat-system

## 1. Cron Service
OpenClaw 的 Cron 服務允許使用者安排定時執行的任務。

### 核心功能：
- **Schedule Types**: 支援 `every` (間隔)、`cron` (標準 cron 表示式) 與 `at` (特定時間)。
- **Payload Types**: 
    - `systemEvent`: 向 Session 注入系統訊息。
    - `agentTurn`: 自動啟動一個 Agent Run 並將結果傳送至指定頻道。
- **Persistence**: 任務清單儲存在 `cron.json` 中，服務重啟後會自動恢復。

## 2. Heartbeat 機制
Heartbeat 是系統的主動檢查機制，確保 Agent 不只是被動回應，還能主動觀察環境。

### 運作原理：
- **Runner**: 背景循環執行，預設頻率由配置決定。
- **HEARTBEAT.md**: 使用者可以在此檔案中加入任務清單，Runner 會讀取並執行。
- **Proactive Behavior**: 當發現異常、重要郵件或定時任務到期時，Agent 會主動發送訊息給使用者。

## 3. 關鍵檔案
- `src/gateway/server-cron.ts`: Cron 服務的 API 實現。
- `src/infra/cron/store.ts`: 處理 Cron 任務的持久化。
- `src/infra/heartbeat-runner.ts`: 負責 Heartbeat 的主循環。

---
*Source: ref/openclaw/src/*
