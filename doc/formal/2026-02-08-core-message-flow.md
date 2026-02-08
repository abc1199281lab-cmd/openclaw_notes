# 2026-02-08-core-message-flow

## Overview
本文詳細分析 OpenClaw 從接收訊息到回傳回覆的完整執行路徑。

## Step-by-Step Flow

### 1. Inbound (接收)
- **Channel Entry**: 訊息由各個 Channel（如 Telegram, WhatsApp）進入。
- **Normalization**: `BaseChannel` 將原始訊息格式化為 `MsgContext`。

### 2. Gateway Dispatch (閘道分發)
- **Endpoint**: `src/gateway/server-methods/chat.ts` 的 `chat.send` 處理請求。
- **Session Resolution**: 根據 `sessionKey` 載入 Session 狀態。
- **Dispatching**: 呼叫 `dispatchInboundMessage`（於 `src/auto-reply/dispatch.ts`）。

### 3. Logic & Routing (邏輯與路由)
- **Reply Resolution**: `getReplyFromConfig`（於 `src/auto-reply/reply/get-reply.ts`）決定使用的 Agent 與模型。
- **Directive Handling**: 解析訊息中的指令（如 `/think`, `/reset`）。
- **Session Persistence**: 更新 Session 歷史紀錄與狀態。

### 4. Agent Execution (代理執行)
- **Runner**: `runReplyAgent` 呼叫 `pi-embedded-runner`。
- **Prompt Building**: 組裝 System Prompt、Session 歷史與 User 訊息。
- **Model Call**: 向 LLM Provider 發送請求。

### 5. Outbound (回傳)
- **Streaming/Batching**: LLM 的回覆透過 `ReplyDispatcher` 即時回傳。
- **Persistence**: Assistant 的回覆被寫回 Session 紀錄。
- **Channel Delivery**: 訊息經由原始 Channel 發回給使用者。

## Key Code Locations
- `src/gateway/server-methods/chat.ts`: 進入點。
- `src/auto-reply/reply/get-reply.ts`: 核心路由邏輯。
- `src/agents/pi-embedded-runner.ts`: Agent 執行核心。

---
*Source: ref/openclaw/src/*
