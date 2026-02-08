# 2026-02-08-security-sandbox-deep-dive

## 1. Security Architecture
OpenClaw 的安全性建立在「最低權限」與「隔離執行」的原則之上。

### 核心安全機制：
- **Tool Policy**: 透過配置檔案限制 Agent 可使用的工具。未在允許清單中的工具呼叫會被 Gateway 攔截。
- **Approval Flow**: 敏感操作（如 `exec` 特定指令）可設定為需要人工審核 (Manual Approval)。
- **Token/Password Auth**: 客戶端連接 Gateway 需要通過身分驗證。

## 2. Sandbox (沙盒執行)
為了安全地執行生成的程式碼或系統指令，OpenClaw 支援 Docker 沙盒。

### 沙盒運作方式：
- **Docker Integration**: `src/agents/sandbox.ts` 管理 Docker 容器的生命週期。
- **Workspace Mounting**: 將特定的工作目錄掛載到容器內，限制 Agent 存取主機檔案系統。
- **Network Isolation**: 容器可以配置為無網路或受限網路。
- **Pty Support**: 支援虛擬終端 (PTY) 以進行互動式指令執行。

## 3. 關鍵檔案
- `src/agents/tool-policy.ts`: 定義工具執行策略。
- `src/agents/sandbox.ts`: Docker 沙盒管理邏輯。
- `src/gateway/exec-approval-manager.ts`: 審核請求管理器。

---
*Source: ref/openclaw/src/*
