# 2026-02-08-plugin-sdk-deep-dive

## 1. Plugin SDK 概論
OpenClaw 的 `plugin-sdk` 是為了讓開發者能夠無縫擴充系統功能而設計的工具包。它整合了類型定義（Types）、配置模式（Schemas）以及常用的工具函式，使得開發第三方插件變得標準化。

SDK 的核心入口位於 `src/plugin-sdk/index.ts`，它匯出了系統中所有可供擴充的介面。

## 2. 插件類型 (Plugin Types)

### A. Channel Plugin (通訊平台插件)
這是最常見的插件類型，用於支援新的通訊平台（如 LINE, MS Teams）。
- **`ChannelPlugin` 介面**：定義了處理登入（Setup）、發送訊息（Outbound）、接收訊息（Inbound）、群組管理以及安全性策略的介面。
- **`ChannelConfigSchema`**：基於 Zod 的配置定義，確保使用者在 `openclaw.mjs` 中的設定符合規範。

### B. OpenClaw Plugin (通用系統插件)
用於擴充 Gateway 的功能，例如：
- **自定義 HTTP 路由**：透過 `registerPluginHttpRoute` 加入新的 Web API 終端點。
- **背景服務 (Services)**：定義 `OpenClawPluginService` 執行定時背景任務。
- **日誌傳輸 (Log Transports)**：透過 `registerLogTransport` 將系統日誌發送到外部服務。

## 3. 核心開發組件

### 配置與驗證
- **Zod Schemas**：SDK 提供了許多內建的 Schema（如 `DmPolicySchema`, `ToolPolicySchema`），開發者可以直接復用來構建插件配置。
- **Config Helpers**：簡化了配置的載入、合併與遷移邏輯。

### 互動與提示
- **`WizardPrompter`**：讓插件在 `openclaw onboard` 或 `openclaw login` 期間能與使用者進行互動式提示。
- **`ChannelOnboardingAdapter`**：定義插件在引導流程（Onboarding）中的行為。

### 訊息處理
- **`MsgContext`**：插件需要將平台特定的訊息包裝成此上下文對象，以便 Agent 處理。
- **`ReplyPayload`**：定義了 Agent 回覆的結構（文字、圖片、工具呼叫等）。

## 4. 關鍵檔案分析
- `src/plugin-sdk/index.ts`: 所有的 SDK 匯出點。
- `src/channels/plugins/types.plugin.ts`: Channel 插件的具體介面定義。
- `src/plugins/types.ts`: 通用插件系統的介面定義。

---
*Source: ref/openclaw/src/plugin-sdk/*
