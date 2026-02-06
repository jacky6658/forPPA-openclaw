# 課程補充資料 - 安裝前必讀

> 📚 本資料夾包含 OpenClaw 完整安裝與使用教學文件  
> ⚠️ **請在開始安裝前，務必先閱讀本文件**

---

## 📋 資料夾內容說明

本資料夾包含以下 5 份文件，建議按照順序閱讀：

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

### 2️⃣ [錯誤排除_telegram_gateway_ui_從錯誤到解決.md](./錯誤排除_telegram_gateway_ui_從錯誤到解決.md)
**必讀優先級：⭐⭐⭐⭐**

- **用途**：安裝後最常遇到的錯誤與解決方案
- **適用對象**：安裝過程中遇到問題的學員
- **重點內容**：
  - Telegram 設定路徑改版說明（`telegram.*` → `channels.telegram.*`）
  - Gateway 啟動失敗的排除方法
  - UI 連線失敗的解決步驟

**建議閱讀時機**：
- 安裝前先快速瀏覽，了解常見問題
- 遇到錯誤時立即參考

---

### 3️⃣ [OpenClaw-終端機指令大全.md](./OpenClaw-終端機指令大全.md)
**必讀優先級：⭐⭐⭐**

- **用途**：OpenClaw 所有終端機指令的完整參考
- **適用對象**：需要操作 OpenClaw 的學員
- **重點內容**：
  - 最常用的 3 個指令
  - 狀態檢查、Gateway 管理、Logs 查看
  - 快速排錯 SOP

**建議閱讀時機**：
- 安裝完成後，開始使用時參考
- 遇到問題時查詢指令用法

---

### 4️⃣ [gog-setup-and-usage.md](./gog-setup-and-usage.md)
**必讀優先級：⭐⭐**

- **用途**：Google Workspace CLI (gog) 的安裝與使用教學
- **適用對象**：需要使用 Google 服務整合功能的學員
- **重點內容**：
  - gog 是什麼？能做什麼？
  - OAuth 憑證設定流程
  - Gmail、Calendar、Drive 等服務的操作指令

**建議閱讀時機**：
- 需要整合 Google 服務時再閱讀
- 非必要功能，可稍後學習

---

### 5️⃣ [AI學習能力說明.md](./AI學習能力說明.md)
**必讀優先級：⭐⭐**

- **用途**：解釋 OpenClaw AI 助理的學習與記憶機制
- **適用對象**：想深入了解 AI 運作原理的學員
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

在開始安裝前，請確認：

1. **作業系統**：macOS（本教學僅適用 Mac）
2. **管理員權限**：你的使用者帳號必須具備 **Administrator（管理員）** 權限
   - 驗證方式：在 Terminal 執行 `sudo -v`，能成功輸入密碼即代表有權限
3. **網路連線**：確保可正常連網（會下載 Homebrew、Node.js、OpenClaw 等）
4. **終端機**：使用 **Terminal.app**（應用程式 → 工具程式 → Terminal）
   - ⚠️ 不要在非互動式環境執行（如某些 IDE 的非 TTY 視窗）

---

### 📝 安裝流程建議順序

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

---

## 🚨 常見問題快速索引

### 安裝階段

| 問題 | 參考文件 | 章節 |
|------|---------|------|
| sudo 密碼輸入後沒反應 | OpenClaw-Mac-安裝教學.md | 第 5 節 |
| 安裝過程很久（是否卡住？） | OpenClaw-Mac-安裝教學.md | 第 2 節 |
| Xcode Command Line Tools 需要更新 | OpenClaw-Mac-安裝教學.md | 第 6 節 |
| 安裝後 openclaw 指令找不到 | OpenClaw-Mac-安裝教學.md | 第 7 節 |

### 設定階段

| 問題 | 參考文件 | 章節 |
|------|---------|------|
| Telegram Token 設定失敗 | 錯誤排除_telegram_gateway_ui_從錯誤到解決.md | 第 1 節 |
| Gateway 啟動失敗 | 錯誤排除_telegram_gateway_ui_從錯誤到解決.md | 第 3 節 |
| UI 顯示拒絕連線 | 錯誤排除_telegram_gateway_ui_從錯誤到解決.md | 第 2 節 |

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
   - 執行 `openclaw doctor --fix` 自動修復常見問題
   - 參考錯誤排除文件中的快速排查清單
   - 截圖或複製 Terminal 最後 20 行錯誤訊息，尋求協助

4. **文件版本**：
   - 本資料夾文件適用於 OpenClaw 2026.2.x 版本
   - 若使用其他版本，部分指令或設定路徑可能略有差異

---

## 📞 需要協助？

如果在安裝過程中遇到問題：

1. **先自行排查**：
   - 執行 `openclaw status --all`
   - 執行 `openclaw doctor --fix`
   - 參考錯誤排除文件

2. **準備資訊**：
   - Terminal 最後 20-30 行輸出（截圖或文字）
   - 執行 `openclaw status --all` 的完整輸出
   - 說明你執行到哪個步驟

3. **尋求協助**：
   - 提供上述資訊給講師或助教
   - 清楚描述問題現象與已嘗試的解決方法

---

## 📚 延伸閱讀

安裝完成後，建議繼續學習：

- **進階設定**：參考 [OpenClaw-終端機指令大全.md](./OpenClaw-終端機指令大全.md) 中的 Config 設定章節
- **Google 服務整合**：參考 [gog-setup-and-usage.md](./gog-setup-and-usage.md)
- **AI 運作原理**：參考 [AI學習能力說明.md](./AI學習能力說明.md)

---

**最後更新**：2026-02-06  
**適用版本**：OpenClaw 2026.2.x

祝安裝順利！🎉
