# 2026-02-08-bus-routing-deep-dive

## 1. 內部匯流排 (Internal Bus)
OpenClaw 使用一個中央匯流排來處理系統內部的事件通訊。這使得不同的組件（如 Channel, Gateway, Agent）能夠以鬆散耦合的方式進行互動。

### 核心特性：
- **異步事件**: 所有的訊息與狀態變更都封裝為事件（Events）。
- **訂閱機制**: 組件可以訂閱特定的事件類型（如 `chat`, `presence`, `voicewake.changed`）。
- **廣播功能**: Gateway 可以將事件廣播給所有連接的客戶端。

## 2. 路由機制 (Routing)
路由決定了訊息該流向哪個 Session 或 Agent。

### 路由邏輯：
- **Session Key**: 每個對話都有一個唯一的 `sessionKey`。路由根據此 Key 尋找對應的 Session 實例。
- **Channel Mapping**: 訊息進入時，Gateway 會識別其來源 Channel，並在回覆時確保訊息循原路返回。
- **Sub-agent Routing**: 子代理的訊息會被路由到專門的子代理 Session，並在完成後將結果送回父 Session。

## 3. 關鍵檔案
- `src/infra/agent-events.ts`: 定義 Agent 相關的事件結構。
- `src/gateway/server-broadcast.ts`: 處理 Gateway 的廣播邏輯。
- `src/routing/session-key.ts`: 處理 Session Key 的規範化與解析。

---
*Source: ref/openclaw/src/*
