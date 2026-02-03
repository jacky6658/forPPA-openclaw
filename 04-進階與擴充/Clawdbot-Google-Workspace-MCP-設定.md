# Clawdbot + Google Workspace MCP 安裝與設定教學

> 目標：讓 Clawdbot 可以用自然語言操作  
> Gmail / Google Calendar / Google Drive / Google Docs / Google Sheets

---

## 📖 閱讀前必讀：這是什麼、何時用、有必要嗎？

### 這是什麼？
本文件是 **Clawdbot 與 Google Workspace 整合教學**，讓您的 Bot 能夠：
- 📧 用自然語言操作 **Gmail**（寄信、讀信、搜尋郵件）
- 📅 操作 **Google Calendar**（建立行程、查詢行事曆）
- 📁 操作 **Google Drive**（上傳、下載、搜尋檔案）
- 📝 操作 **Google Docs** 和 **Google Sheets**（建立、編輯文件）

### 何時需要看這份文件？
| 情境 | 是否需要 | 說明 |
|------|---------|------|
| **想讓 Bot 幫我收發 Gmail、管理行事曆** | ✅ 需要 | 依本文完整設定 MCP 與 Google 授權 |
| **想讓 Bot 操作 Google Drive、Docs、Sheets** | ✅ 需要 | 依本文完整設定 |
| **只需要基本對話功能、不需要 Google 整合** | ❌ 不需要 | 基本 Clawdbot 功能已足夠 |
| **還沒安裝 Clawdbot** | ❌ 暫不需要 | 先完成 Clawdbot 基本安裝，之後需要整合時再回來看 |

### 技術層級
- 🟡 **中級**：需要執行多個指令、設定 Google Cloud 專案、處理 OAuth 授權
- 預計時間：30-60 分鐘

### 必要嗎？
- **如果您需要 Bot 幫忙處理 Gmail、行事曆、雲端硬碟** → 必要，這是唯一的整合指南
- **如果您只需要對話、記憶、模型切換** → 不必要，無須整合 Google Workspace

---

## 🧱 架構說明

Clawdbot  
→ mcpporter（MCP 管理器）  
→ google-workspace-mcp（工具伺服器）  
→ Google APIs

---

## ✅ 前置條件

請先確認已安裝：

- Node.js
- npm
- Clawdbot

---

## ✅ Step 1：安裝 Google Workspace MCP

```bash
npm install -g @presto-ai/google-workspace-mcp
```

---

## ✅ Step 2：第一次啟動並完成 Google OAuth 登入

```bash
npx @presto-ai/google-workspace-mcp
```

1. 會自動打開瀏覽器
2. 登入 Google 帳號
3. 點擊允許
4. 看到「Authentication successful」即完成

完成後可關閉該視窗。

---

## ✅ Step 3：進入 Clawdbot 設定

```bash
clawdbot configure
```

---

## ✅ Step 4：在 Clawdbot 內安裝 mcpporter

在設定流程中：

1. 選擇：Skills  
2. 勾選：Install missing skill dependencies  
3. 勾選：mcpporter  
4. 安裝完成

---

## ✅ Step 5：註冊 Google Workspace MCP

在終端機執行：

```bash
mcpporter config add google-workspace \
  --command "npx" \
  --arg "-y" \
  --arg "@presto-ai/google-workspace-mcp" \
  --scope home
```

---

## ✅ Step 6：啟動 Clawdbot

```bash
clawdbot
```

---

## ✅ Step 7：測試

在 Clawdbot 輸入：

- 幫我查 Gmail 未讀信
- 幫我新增明天下午三點行事曆

---

## 🧪 檢查 MCP 是否存在

```bash
cat ~/.mcpporter/mcpporter.json
```

---

## ⚠️ 注意

- 不要長期手動執行 npx @presto-ai/google-workspace-mcp
- 應由 Clawdbot 自動呼叫

---

## 🎉 完成

你的 Clawdbot 已正式連接 Google Workspace！
