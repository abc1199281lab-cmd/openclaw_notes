# 2026-02-08-openclaw-high-level-architecture

## Overview
OpenClaw 是一個高度模組化的 AI Agent 框架，其核心設計目標是提供跨平台的通訊（Multi-channel）與強大的工具執行能力（Tool Use）。

## Core Components

### 1. Gateway & Daemon
- **Gateway**: 作為系統的通訊中樞，負責處理所有進出訊息的路由。
- **Daemon**: 系統的背景服務，管理專案狀態、Session 持久化與 Cron 任務。

### 2. Bus & Routing
- 使用內部事件匯流排（Bus）實現組件間的解耦。
- 路由機制確保訊息能準確傳遞至正確的 Session 或 Agent。

### 3. Agent System
- **Core Agent**: 處理邏輯、調用工具、維護上下文。
- **Skills**: 封裝了特定的功能（如 Web Search, File I/O, GitHub API）。
- **Sub-agents**: 支援衍生子代理執行耗時任務。

### 4. Communication Channels
- 支援 Telegram, Signal, WhatsApp, WebChat, Discord 等。
- 統一的 `BaseChannel` 介面，簡化了新平台的擴展。

### 5. Memory & Context
- **Short-term**: Session 歷史紀錄與上下文訊息。
- **Long-term**: `MEMORY.md` 與 `memory/` 資料夾中的結構化知識。

## Message Flow (High-level)
1. **Input**: 使用者透過 Telegram 發送訊息。
2. **Channel**: Telegram Channel 接收並轉換為統一格式。
3. **Gateway**: Gateway 接收訊息，根據 `sessionKey` 尋找或建立 Session。
4. **Agent**: Agent 接收上下文，決定執行工具或直接回覆。
5. **Output**: 回覆經由 Gateway 與 Channel 傳回使用者。

---
*Source: ref/openclaw/src/*
