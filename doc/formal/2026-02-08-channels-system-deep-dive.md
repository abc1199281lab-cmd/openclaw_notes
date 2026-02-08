# 2026-02-08-channels-system-deep-dive

## 1. Channel 角色
Channels 是 OpenClaw 與外部通訊平台（Telegram, Signal, WhatsApp, Discord 等）的介面。每個 Channel 負責將特定平台的協議轉換為 OpenClaw 內部的統一格式。

## 2. 核心組件
- **`BaseChannel`**: 所有 Channel 插件的基類，定義了發送訊息、處理反應、輪詢等標準行為。
- **Channel Manager (`src/gateway/server-channels.ts`)**: 負責載入與管理各個 Channel 實例。
- **Reply Prefix**: 每個 Channel 可以設定回覆的前綴（如在群組中標註使用者）。

## 3. 各平台實現簡述
- **Telegram**: 使用 `telegraf` 或類似函式庫。
- **Signal**: 通常透過 `signal-cli` 的 REST API 介面。
- **WhatsApp**: 基於 `baileys` 或類似的 WebSocket 庫實現。
- **WebChat**: OpenClaw 自帶的內部通訊介面。

## 4. 訊息接收流程 (Inbound)
1. 第三方 Webhook 或 Long Polling 接收到原始訊息。
2. Channel 解析訊息主體、發送者、群組資訊。
3. 封裝成 `MsgContext` 並交由 Gateway 分發。

---
*Source: ref/openclaw/src/channels/*
