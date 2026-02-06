# 課程補充資料 - 安裝前必讀

> 📚 本資料夾包含 OpenClaw 完整安裝與使用教學文件  
> ⚠️ **請在開始安裝前，務必先閱讀本文件**

---

## 📋 資料夾內容說明

本資料夾包含以下 7 份文件，請根據你的作業系統選擇對應的安裝教學：

---

## 🍎 macOS 使用者必讀

### 1️⃣ [OpenClaw-Mac-安裝教學.md](./OpenClaw-Mac-安裝教學.md)
**必讀優先級：⭐⭐⭐⭐⭐**

- **用途**：OpenClaw 在 macOS 上的一鍵安裝流程
- **適用對象**：所有 Mac 使用者
- **重點內容**：
  - 一鍵安裝腳本執行方式
  - 安裝過程中的互動提示說明
  - 安裝完成後的驗收步驟
  - 常見問題排除（sudo 權限、Xcode Command Line Tools 等）

**建議閱讀時機**：開始安裝前先完整閱讀一遍

---

### 2️⃣ [OpenClaw-終端機指令大全.md](./OpenClaw-終端機指令大全.md)
**必讀優先級：⭐⭐⭐**

- **用途**：OpenClaw macOS 終端機指令的完整參考
- **適用對象**：Mac 使用者，需要操作 OpenClaw
- **重點內容**：
  - 最常用的 3 個指令
  - 狀態檢查、Gateway 管理、Logs 查看
  - 快速排錯 SOP
  - zsh/bash 指令範例

**建議閱讀時機**：
- 安裝完成後，開始使用時參考
- 遇到問題時查詢指令用法

---

## 🪟 Windows 使用者必讀

### 3️⃣ [OpenClaw-Windows-安裝教學.md](./OpenClaw-Windows-安裝教學.md)
**必讀優先級：⭐⭐⭐⭐⭐**

- **用途**：OpenClaw 在 Windows 10/11 上的安裝流程
- **適用對象**：所有 Windows 使用者
- **重點內容**：
  - Node.js 安裝步驟
  - OpenClaw 安裝與設定
  - Gateway 啟動方式（前台/背景）
  - Task Scheduler 自動啟動設定
  - Windows 常見問題排除

**建議閱讀時機**：開始安裝前先完整閱讀一遍

---

### 4️⃣ [OpenClaw-Windows-終端機指令大全.md](./OpenClaw-Windows-終端機指令大全.md)
**必讀優先級：⭐⭐⭐**

- **用途**：OpenClaw Windows PowerShell 指令的完整參考
- **適用對象**：Windows 使用者，需要操作 OpenClaw
- **重點內容**：
  - PowerShell 指令範例
  - Gateway 管理（Task Scheduler）
  - 日誌查看與偵錯
  - Windows 專用排錯方法
  - 實用 PowerShell 腳本

**建議閱讀時機**：
- 安裝完成後，開始使用時參考
- 遇到問題時查詢指令用法

---

## 🔧 跨平台通用文件

### 5️⃣ [錯誤排除_telegram_gateway_ui_從錯誤到解決.md](./錯誤排除_telegram_gateway_ui_從錯誤到解決.md)
**必讀優先級：⭐⭐⭐⭐**

- **用途**：安裝後最常遇到的錯誤與解決方案（Mac/Windows 通用）
- **適用對象**：安裝過程中遇到問題的學員
- **重點內容**：
  - Telegram 設定路徑改版說明（`telegram.*` → `channels.telegram.*`）
  - Gateway 啟動失敗的排除方法
  - UI 連線失敗的解決步驟

**建議閱讀時機**：
- 安裝前先快速瀏覽，了解常見問題
- 遇到錯誤時立即參考

---

### 6️⃣ [gog-setup-and-usage.md](./gog-setup-and-usage.md)
**必讀優先級：⭐⭐**

- **用途**：Google Workspace CLI (gog) 的安裝與使用教學
- **適用對象**：需要使用 Google 服務整合功能的學員（Mac/Windows）
- **重點內容**：
  - gog 是什麼？能做什麼？
  - OAuth 憑證設定流程
  - Gmail、Calendar、Drive 等服務的操作指令

**建議閱讀時機**：
- 需要整合 Google 服務時再閱讀
- 非必要功能，可稍後學習

---

### 7️⃣ [AI學習能力說明.md](./AI學習能力說明.md)
**必讀優先級：⭐⭐**

- **用途**：解釋 OpenClaw AI 助理的學習與記憶機制
- **適用對象**：想深入了解 AI 運作原理的學員（Mac/Windows）
- **重點內容**：
  - AI 沒有真正的自主學習能力
  - 透過檔案系統實現「記憶」的機制
  - 如何讓 AI「學習」和「記住」資訊

**建議閱讀時機**：
- 對 AI 運作原理有興趣時閱讀
- 想優化 AI 行為時參考

---

## ⚠️ 安裝前必讀事項

### ✅ 系統需求檢查

在開始安裝前，請根據你的作業系統確認：

#### 🍎 macOS 使用者

1. **作業系統**：macOS（建議 macOS 10.15 或更新版本）
2. **管理員權限**：你的使用者帳號必須具備 **Administrator（管理員）** 權限
   - 驗證方式：在 Terminal 執行 `sudo -v`，能成功輸入密碼即代表有權限
3. **網路連線**：確保可正常連網（會下載 Homebrew、Node.js、OpenClaw 等）
4. **終端機**：使用 **Terminal.app**（應用程式 → 工具程式 → Terminal）
   - ⚠️ 不要在非互動式環境執行（如某些 IDE 的非 TTY 視窗）

#### 🪟 Windows 使用者

1. **作業系統**：Windows 10 (版本 1809 或更新) 或 Windows 11
2. **系統資源**：
   - 至少 4GB RAM
   - 2GB 可用硬碟空間
3. **管理員權限**：建議以系統管理員身份執行 PowerShell（避免權限問題）
4. **網路連線**：確保可正常連網（會下載 Node.js、OpenClaw 等）
5. **PowerShell**：Windows 10/11 內建 PowerShell（建議使用 PowerShell 5.1+ 或 PowerShell 7+）

---

### 📝 安裝流程建議順序

#### 🍎 macOS 使用者流程

1. **第一步**：閱讀 [OpenClaw-Mac-安裝教學.md](./OpenClaw-Mac-安裝教學.md)
   - 完整了解安裝流程與注意事項

2. **第二步**：快速瀏覽 [錯誤排除_telegram_gateway_ui_從錯誤到解決.md](./錯誤排除_telegram_gateway_ui_從錯誤到解決.md)
   - 了解常見問題，避免重複踩坑

3. **第三步**：執行安裝
   - 按照安裝教學的步驟操作
   - 遇到問題立即參考錯誤排除文件

4. **第四步**：驗收安裝結果
   - 執行 `openclaw --help`
   - 執行 `openclaw gateway status`
   - 執行 `openclaw dashboard` 打開控制介面

5. **第五步**：參考 [OpenClaw-終端機指令大全.md](./OpenClaw-終端機指令大全.md)
   - 學習常用指令
   - 收藏快速排錯 SOP

#### 🪟 Windows 使用者流程

1. **第一步**：閱讀 [OpenClaw-Windows-安裝教學.md](./OpenClaw-Windows-安裝教學.md)
   - 完整了解安裝流程與注意事項
   - 特別注意 Node.js 安裝步驟

2. **第二步**：快速瀏覽 [錯誤排除_telegram_gateway_ui_從錯誤到解決.md](./錯誤排除_telegram_gateway_ui_從錯誤到解決.md)
   - 了解常見問題，避免重複踩坑

3. **第三步**：執行安裝
   - 先安裝 Node.js（如尚未安裝）
   - 再安裝 OpenClaw
   - 執行設定精靈 `openclaw configure`
   - 遇到問題立即參考錯誤排除文件

4. **第四步**：驗收安裝結果
   - 執行 `openclaw --version`
   - 執行 `openclaw gateway status`
   - 在瀏覽器打開 `http://localhost:18789` 查看 Dashboard

5. **第五步**：參考 [OpenClaw-Windows-終端機指令大全.md](./OpenClaw-Windows-終端機指令大全.md)
   - 學習 PowerShell 指令
   - 了解 Windows 專用的 Gateway 啟動方式（Task Scheduler）

---

## 🚨 常見問題快速索引

### 🍎 macOS 安裝階段

| 問題 | 參考文件 | 章節 |
|------|---------|------|
| sudo 密碼輸入後沒反應 | OpenClaw-Mac-安裝教學.md | 第 5 節 |
| 安裝過程很久（是否卡住？） | OpenClaw-Mac-安裝教學.md | 第 2 節 |
| Xcode Command Line Tools 需要更新 | OpenClaw-Mac-安裝教學.md | 第 6 節 |
| 安裝後 openclaw 指令找不到 | OpenClaw-Mac-安裝教學.md | 第 7 節 |

### 🪟 Windows 安裝階段

| 問題 | 參考文件 | 章節 |
|------|---------|------|
| 安裝時出現「拒絕存取」錯誤 | OpenClaw-Windows-安裝教學.md | Q1 |
| openclaw 指令找不到 | OpenClaw-Windows-安裝教學.md | Q2 |
| Node.js 版本太舊 | OpenClaw-Windows-安裝教學.md | Q3 |
| Gateway Port 被佔用 | OpenClaw-Windows-安裝教學.md | Q4 |
| PowerShell 執行政策錯誤 | OpenClaw-Windows-終端機指令大全.md | 常見問題 1 |

### 🔧 設定階段（Mac/Windows 通用）

| 問題 | 參考文件 | 章節 |
|------|---------|------|
| Telegram Token 設定失敗 | 錯誤排除_telegram_gateway_ui_從錯誤到解決.md | 第 1 節 |
| Gateway 啟動失敗 | 錯誤排除_telegram_gateway_ui_從錯誤到解決.md | 第 3 節 |
| UI 顯示拒絕連線 | 錯誤排除_telegram_gateway_ui_從錯誤到解決.md | 第 2 節 |
| Telegram Bot 無法連線 | OpenClaw-Windows-安裝教學.md | Q5 |
| API Key 無效 | OpenClaw-Windows-安裝教學.md | Q6 |

---

## 💡 重要提醒

1. **Token 安全**：
   - Telegram Bot Token、Google OAuth credentials 等敏感資訊
   - ⚠️ **絕對不要**貼到群組、公開論壇或任何公開場所
   - 這些資訊等同密碼，洩露會造成安全風險

2. **安裝時間**：
   - 第一次安裝可能需要 **10-30 分鐘**
   - 這是正常現象，請耐心等待
   - 看到 `Installing...`、`Downloading...`、`Pouring...` 等訊息代表正在進行中

3. **遇到問題時**：
   - 先執行 `openclaw status --all` 查看完整狀態
   - **Mac**：執行 `openclaw doctor --fix` 自動修復常見問題
   - **Windows**：參考 Windows 終端機指令大全中的排錯方法
   - 參考錯誤排除文件中的快速排查清單
   - 截圖或複製終端機（Terminal/PowerShell）最後 20 行錯誤訊息，尋求協助

4. **文件版本**：
   - 本資料夾文件適用於 OpenClaw 2026.2.x 版本
   - 若使用其他版本，部分指令或設定路徑可能略有差異

---

## 📞 需要協助？

如果在安裝過程中遇到問題：

1. **先自行排查**：
   - 執行 `openclaw status --all`
   - 執行 `openclaw doctor --fix`（Mac）或參考 Windows 終端機指令大全的排錯方法
   - 參考錯誤排除文件

2. **準備資訊**：
   - **Mac**：Terminal 最後 20-30 行輸出（截圖或文字）
   - **Windows**：PowerShell 最後 20-30 行輸出（截圖或文字）
   - 執行 `openclaw status --all` 的完整輸出
   - 說明你的作業系統（Mac/Windows）和執行到哪個步驟

3. **尋求協助**：
   - 提供上述資訊給講師或助教
   - 清楚描述問題現象與已嘗試的解決方法
   - 如果是 Windows，請註明 PowerShell 版本

---

## 📚 延伸閱讀

安裝完成後，建議繼續學習：

### 🍎 macOS 使用者

- **進階設定**：參考 [OpenClaw-終端機指令大全.md](./OpenClaw-終端機指令大全.md) 中的 Config 設定章節
- **Google 服務整合**：參考 [gog-setup-and-usage.md](./gog-setup-and-usage.md)
- **AI 運作原理**：參考 [AI學習能力說明.md](./AI學習能力說明.md)

### 🪟 Windows 使用者

- **PowerShell 指令**：參考 [OpenClaw-Windows-終端機指令大全.md](./OpenClaw-Windows-終端機指令大全.md)
- **Gateway 自動啟動**：參考 [OpenClaw-Windows-終端機指令大全.md](./OpenClaw-Windows-終端機指令大全.md) 中的 Task Scheduler 設定
- **Google 服務整合**：參考 [gog-setup-and-usage.md](./gog-setup-and-usage.md)
- **AI 運作原理**：參考 [AI學習能力說明.md](./AI學習能力說明.md)

---

**最後更新**：2026-02-06  
**適用版本**：OpenClaw 2026.2.x

祝安裝順利！🎉
