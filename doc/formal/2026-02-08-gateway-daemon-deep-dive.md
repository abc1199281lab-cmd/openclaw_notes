# 2026-02-08-gateway-daemon-deep-dive

## 1. Gateway 職責
Gateway 是 OpenClaw 的通訊中樞，主要負責：
- **WebSocket/HTTP Server**: 提供 API 給 UI (Control UI) 與第三方客戶端。
- **Channel 橋接**: 連結不同的通訊平台（Telegram, Signal, WhatsApp）。
- **Request Handling**: 處理來自客戶端的 RPC 請求（如發送訊息、修改配置、列出 Session）。
- **Auth & Security**: 驗證客戶端 Token 與裝置身分。

## 2. Daemon 運作機制
Daemon 是背景運行的服務，確保系統持久性：
- **Service Lifecycle**: 負責啟動與重啟 Gateway。
- **Session Persistence**: 定期將記憶體中的 Session 狀態同步至 `.jsonl` 檔案。
- **Cron Service**: 執行定時任務（如每日提醒、定時狀態檢查）。
- **Signal Handling**: 監聽系統訊號（如 SIGUSR1）以執行熱重啟（Hot Restart）。

## 3. 關鍵檔案分析
- `src/gateway/server.impl.ts`: Gateway 啟動與初始化邏輯。
- `src/gateway/server-methods.ts`: 定義了所有的 RPC 方法映射。
- `src/infra/heartbeat-runner.ts`: 驅動 Heartbeat 機制。
- `src/infra/restart.ts`: 處理 Gateway 的重啟邏輯。

---
*Source: ref/openclaw/src/gateway/*
