# OpenClaw 本機安裝與安全重裝指南

本文件旨在提供一份在**本機環境**（例如 macOS、Linux 個人電腦或 Windows WSL2）安裝 OpenClaw 的完整指南，並詳細說明如何在保護現有記憶與知識的前提下，安全地進行備份、刪除與重新安裝。所有步驟均基於 OpenClaw 官方文件 [1] [2] [3]。

---

## 請先確認您的情境（選一條路就好）

**重要**：下面有**兩條不同的路**，請依您的情況**只選一條**，不要兩條都照著做，否則會變成「剛裝好又卸載」的困惑。

| 情境 | 您要做的事 | 請看的章節 | 不要做什麼 |
|------|------------|------------|------------|
| **A) 第一次安裝** | 電腦還沒裝過 OpenClaw，要從零裝好 | 先做 **一、本機安裝前的準備**，再做 **二、本機安裝流程** → 做完就結束 | **不要**再去做「三」；「三」是給已經裝過、要重裝的人用的 |
| **B) 要重裝（保留記憶）** | 已經裝過，想卸載後再裝一次並還原備份 | 先做 **一**（若 Node 已裝可略過），然後**只做「三」的五個步驟**（備份 → 卸載 → 重裝 → 還原 → 驗證） | **不要**先做「二」再來做「三」；重裝時「三」裡面自己會包含「重新安裝」這一步 |

**一句話**：  
- **第一次安裝** → 看「一」+「二」就好。  
- **要重裝** → 看「一」（可略）+「三」就好；「三」是一整套（先備份、再卸載、再重裝、再還原），不是接在「二」後面的下一步。

---

## 一、本機安裝前的準備

### 1. 系統需求

在開始安裝前，請確保您的本機環境符合以下基本要求：

| 元件 | 要求 |
| :--- | :--- |
| **作業系統** | macOS、Linux 或 Windows (透過 WSL2) |
| **Node.js** | 版本 >= 22 |

> **Windows 使用者注意**：強烈建議使用 Windows Subsystem for Linux 2 (WSL2) 來模擬 Linux 環境，以獲得最佳的相容性與體驗。

### 2. 安裝 Node.js

如果您的系統尚未安裝 Node.js，建議使用 `nvm` (Node Version Manager) 來安裝，以便管理多個 Node.js 版本。

```bash
# 1. 安裝 nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# 2. 讓 nvm 指令生效 (或重開終端機)
# macOS 預設使用 zsh，請用：
source ~/.zshrc
# 若您使用 bash（例如 Linux 或 WSL），請改為：source ~/.bashrc

# 3. 安裝 Node.js v22
nvm install 22

# 4. 設為預設版本並驗證
nvm use 22
nvm alias default 22
node -v # 應顯示 v22.x.x
```

> **若出現「no such file or directory: /Users/user/.bashrc」**  
> 現在 **macOS 預設用的是 zsh，不是 bash**。zsh 讀的是 `~/.zshrc`，不會讀 `~/.bashrc`，所以執行 `source ~/.bashrc` 會報錯。請改為執行 **`source ~/.zshrc`**；或直接**關掉終端機再開一個新視窗**，新視窗會自動載入 `~/.zshrc` 的設定（例如 nvm）。Linux 或 WSL 若使用 bash，則用 `source ~/.bashrc`。  
> **不確定自己用哪個 shell？** 在終端機執行 `echo $SHELL`：若顯示 **`/bin/zsh`** 表示使用 zsh（用 `source ~/.zshrc`）；若顯示 **`/bin/bash`** 表示使用 bash（用 `source ~/.bashrc`）。

---

## 二、OpenClaw 本機安裝流程（給「第一次安裝」的人）

**若您是第一次安裝**，做到這裡就結束。**不要**接著做下面的「三」；「三」是給已經裝過、要重裝的人用的。

官方提供了多種安裝方式，對於大多數使用者而言，**一鍵安裝腳本**是最簡單、最推薦的選擇。

### 方法一：一鍵安裝腳本 (推薦)

此腳本會自動處理 npm 全域安裝並啟動引導設定精靈。

-   **macOS / Linux / WSL2:**
    ```bash
    curl -fsSL https://openclaw.ai/install.sh | bash
    ```

-   **Windows (PowerShell):**
    ```powershell
    iwr -useb https://openclaw.ai/install.ps1 | iex
    ```

安裝完成後，腳本會自動進入下一步的**引導設定精靈 (Onboarding Wizard)**，請依照提示完成 AI 模型、通訊軟體等設定。

### 方法二：手動全域安裝

如果您偏好手動操作，也可以使用 `npm` 或 `pnpm` 進行全域安裝。

```bash
# 使用 npm
npm install -g openclaw@latest

# 安裝後手動啟動引導精靈
openclaw onboard --install-daemon
```

> **疑難排解**：若安裝後執行 `openclaw` 指令出現 `command not found` 錯誤，通常是 npm 的全域路徑未加入系統的 `PATH` 環境變數。請將 `$(npm prefix -g)/bin` 的路徑加入您的 `~/.zshrc` 或 `~/.bashrc` 設定檔中。

### 安裝完成後，接下來要做什麼？

引導精靈結束後，終端機會顯示幾項**建議的下一步**。以下是對應說明，方便您依需要操作：

| 項目 | 說明與指令 |
|------|------------|
| **網頁儀表板 (Web UI)** | 儀表板由 **Gateway** 提供，需先確保 Gateway 有在運行。之後若要開啟網頁操作介面，可執行：<br>`openclaw dashboard --no-open`<br>再在瀏覽器開啟終端機顯示的網址。**預設**為 `http://127.0.0.1:18789/`（埠 18789 為官方預設，每人本機皆同；127.0.0.1 表示本機，只會連到自己這台電腦）。若曾在設定檔中修改 `gateway.port`，網址會變成 `http://127.0.0.1:您設的埠/`。<br>若瀏覽器出現 **「無法連上這個網站」或 127.0.0.1 拒絕連線**，請見下方「若無法連上儀表板」疑難排解。 |
| **工作區備份** | 建議定期備份 agent 的工作區（記憶、設定等），避免重裝或換電腦時遺失。做法可參考本文件**「三、安全備份與重新安裝」**的備份步驟；官方說明：https://docs.openclaw.ai/concepts/agent-workspace |
| **安全性** | 在本機執行 AI agent 有一定風險，建議閱讀**《02-OpenClaw-全方位實作與安全指南》**中的安全章節，並依需要強化設定。官方說明：https://docs.openclaw.ai/security |
| **網路搜尋（選用）** | 若希望 agent 能搜尋網路，需設定 **Brave Search API Key**。可執行：<br>`openclaw configure --section web`<br>依提示啟用 `web_search` 並貼上 API Key。或設定環境變數 `BRAVE_API_KEY`。官方說明：https://docs.openclaw.ai/tools/web |
| **接下來可以做什麼** | 可到 **https://openclaw.ai/showcase** 看看其他人如何運用 OpenClaw，或繼續閱讀同資料夾的 **《02-OpenClaw-全方位實作與安全指南》** 了解進階設定與安全建議。 |

**一句話**：裝完後可先開儀表板 (`openclaw dashboard --no-open`) 試用，再依需要做備份、安全設定與選用功能（如網路搜尋）。

**運行 UI 後的下一步（當儀表板能開、Gateway 顯示 reachable 時）**  
當您已能正常開啟儀表板、且 `openclaw status` 顯示 Gateway reachable 後，建議依序或選做：

1. **確認狀態**：執行 `openclaw status` 或 `openclaw health`，確認 Gateway、Agents、Memory、Channels 皆正常（解讀方式見下方「如何解讀 openclaw status 與 openclaw health」）。
2. **試用儀表板**：在儀表板中試用 Chat、Channels、Sessions 等，確認能與龍蝦對話、頻道（如 Telegram）正常。
3. **做工作區備份**：依上方表格「工作區備份」或本文件「三、安全備份與重新安裝」的備份步驟，備份記憶與設定。
4. **看安全建議**：閱讀 **《02-OpenClaw-全方位實作與安全指南》** 安全章節；可執行 `openclaw security audit` 查看建議。
5. **選填 API / 進階**：若要網路搜尋，執行 `openclaw configure --section web` 設定 Brave Search API Key；或到 https://openclaw.ai/showcase 與 02 文件了解進階用法。

若您想**先給別人檢查龍蝦狀態**：請在終端機執行 `openclaw status`（或 `openclaw status --deep`），將輸出貼上或截圖，即可請人幫您確認是否正常。

**什麼是背景服務？如何安裝？**  
- **背景服務**（daemon／系統服務）是指 **Gateway 由作業系統在背景代為運行**，而不是只在您開著終端機時才跑。  
  - **未安裝時**：需手動執行 `openclaw gateway` 或 `openclaw gateway start`；關掉終端機或登出後，Gateway 可能停止。  
  - **安裝後**：Gateway 由系統管理（macOS 為 **LaunchAgent**、Linux 為 **systemd** 等），**關掉終端機後仍會繼續跑**，**開機後可設定為自動啟動**，儀表板與 Telegram 等頻道可 24/7 連線。  
- **如何安裝 Gateway 為背景服務**  
  1. 在終端機執行：`openclaw gateway install`  
     可選參數：`--port <埠號>`、`--token <token>`、`--force`（覆蓋既有安裝）。  
  2. 安裝完成後執行：`openclaw gateway start`  
  3. 確認：執行 `openclaw status`，若顯示 **Gateway service: LaunchAgent installed · loaded · running (pid xxxxx)** 即表示背景服務已安裝且在跑。  
  4. 之後可用 `openclaw gateway start`、`openclaw gateway stop`、`openclaw gateway restart` 管理服務。  
- 若您**已安裝過**（例如曾執行 `openclaw onboard --install-daemon`），status 可能已顯示 LaunchAgent installed；若要重新安裝或覆蓋，可執行 `openclaw gateway install --force`，再 `openclaw gateway start`。

**模型選擇、驗證與狀態說明**  
- **如何選 model**  
  在 configure 或 model picker 畫面會看到 **Models in /model picker (multi-select)**，表示可**多選**。在要使用的模型左側方框打勾（例如 `openai/gpt-5.1-codex`、`azure-openai-responses/gpt-5`、`anthropic/claude-sonnet-4-5` 等），選完依畫面儲存或確認即可；之後在 OpenClaw 用 `/model` 或模型選擇器時會出現您選的模型。  
- **為什麼會看到亞馬遜（Amazon）？可以不用嗎？**  
  OpenClaw 支援多種供應商（OpenAI、Anthropic、Google、Azure OpenAI、**Amazon Bedrock** 等），**沒有規定一定要用亞馬遜**。若畫面或設定裡出現 Amazon／Bedrock，可能是先前 configure 或設定檔有選到；您可改為只用 OpenAI、Google、Anthropic 等：在 model picker 取消勾選 Bedrock、勾選您要的供應商與模型，或在 `openclaw configure`、設定檔 `~/.openclaw/openclaw.json` 的 `agents.defaults.model` 中改為其他供應商的模型。  
- **「auth missing」是什麼？怎麼解決？**  
  **auth missing** 表示**該模型／供應商尚未在 OpenClaw 裡設定驗證**（API Key 或 OAuth），因此無法實際呼叫該模型。解決方式：執行 `openclaw configure`，找到對應供應商（例如 OpenAI、Anthropic），填入 **API Key** 或依提示完成 **OAuth**，存檔後再選一次該模型；若仍顯示 auth missing，請確認該供應商是否已成功儲存（例如 OpenAI 需到 platform.openai.com 申請 API key 或在 configure 中完成 OAuth）。  
- **有 Claude Max 訂閱但沒有 API key（Anthropic setup-token）**  
  **Claude Max 訂閱**（在 claude.ai 網頁版）與 **Anthropic API key**（在 console.anthropic.com 給開發者用）是兩套不同的驗證方式。若您有買 Claude Max 但沒有 API key，可試用 OpenClaw 的 **Anthropic (setup-token + API key)** 選項：  
  - **Setup-token** 通常是指「用瀏覽器登入 claude.ai 後的 session token」；**理論上有買 Claude Max 就能用**（用您網頁版的 session），但實務上可能有 Anthropic 的限制（如偵測非瀏覽器流量）、額度與網頁版共用、或 token 過期需重新取得等。建議先試試看，若不行或想要獨立 API 額度，再到 console.anthropic.com 申請開發者 API key。  
  - **如何取得 setup-token**：① 在瀏覽器登入 claude.ai（用您的 Claude Max 帳號）；② 按 F12 或右鍵 → 檢查，開啟開發者工具；③ 切到 **Application（應用程式）** → **Cookies** → `https://claude.ai`，找名為 `sessionKey` 或類似的 cookie，複製其 value；或切到 **Network（網路）**，重新整理網頁，找對 claude.ai 的請求，在 **Headers** 裡找 `Cookie:` 或 `Authorization:` 開頭那行，複製 token 部分；④ 在 `openclaw configure` 選 **Anthropic (setup-token + API key)** 時，依提示貼上剛取得的 token。  
  - **若 configure 畫面提示執行 `claude setup-token` 但本機沒有 claude 指令**：表示需先安裝 **Anthropic 的 claude CLI**。安裝步驟：  
    ① 在終端機執行：
    ```bash
    curl -fsSL https://claude.ai/install.sh | bash
    ```
    ② **重要**：安裝完成後，需讓 PATH 生效。執行：
    ```bash
    echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc
    ```
    或**直接關閉終端、開新終端視窗**（推薦，確保環境變數生效）。  
    ③ 在新終端（或執行完 source 後）執行 `claude setup-token`，會產生 token 可複製。  
    ④ 將產生的 token 貼到 OpenClaw configure 畫面的 **Paste Anthropic setup-token** 輸入框。  
    若不想安裝 CLI，也可改用上述「從瀏覽器手動取得 session token」的方式。  
  - **注意**：Session token 會過期（通常幾天到幾週），過期後需重新登入 claude.ai 並取得新 token。  
- **模型描述各段意思（例如「GPT-5-Codex · ctx 391k · reasoning · auth missing」）**  

  | 片段 | 意思 |
  |------|------|
  | **模型名稱**（如 GPT-5-Codex、claude-sonnet-4-5） | 該模型的識別名稱與供應商。 |
  | **ctx 391k**（或 ctx 195k 等） | **上下文長度**：約 39 萬（或 19.5 萬）個 token，表示單一對話中模型能「記住」的輸入＋輸出總量；數字愈大，可處理的長文、長對話愈多。 |
  | **reasoning** | 表示該模型具**推理／思考**能力（例如 chain-of-thought），可能會多花一點時間再回覆。 |
  | **auth missing** | **尚未設定驗證**：該模型／供應商在 OpenClaw 裡還沒有可用的 API Key 或 OAuth，目前無法使用；需在 `openclaw configure` 中補上對應供應商的驗證。 |

- **如何更換預設模型（例如從 Gemini 改成 GPT）**  
  OpenClaw 預設可能使用 Gemini 模型，若您想改用 GPT（或其他模型），有兩種方式：

  **方式一：在 configure 畫面中修改（推薦）**  
  ① 執行 `openclaw configure`，用方向鍵 ↓ 往下捲，找到 **「What model should agents use by default?」** 或類似的模型選擇選項。  
  ② 按 Enter 進入模型清單，會看到所有可用的模型（包括 GPT-4o、GPT-5 等）。  
  ③ 用方向鍵選擇您要的 GPT 模型，按 Enter 確認，繼續完成其他設定項目。  
  ④ 在最後按 Enter 儲存設定。  
  ⚠️ **重要**：改用 GPT 之前，**必須先在 configure 中設定 OpenAI 的驗證**（OAuth 或 API Key），否則會出現 **「auth missing」**，模型無法使用。

  **方式二：手動編輯設定檔**  
  若您已經離開 configure 畫面，也可以直接編輯設定檔：  
  ① 開啟設定檔：`open ~/.openclaw/openclaw.json`（或 `clawdbot.json`）  
  ② 找到這段：
  ```json
  "agents": {
    "defaults": {
      "model": {
        "primary": "google-gemini-cli/gemini-2.5-flash"
      }
    }
  }
  ```
  ③ 改成您要的模型，例如：
  ```json
  "primary": "openai-codex/gpt-5.2"
  ```
  ④ 存檔後重啟 Gateway：
  ```bash
  openclaw gateway stop
  openclaw gateway start
  ```

- **GPT 模型選擇：Codex vs 通用版**  
  OpenAI 的 GPT 有兩種版本，選擇時需考慮您的使用情境：

  | 模型類型 | 適合情境 | 特點 |
  |----------|----------|------|
  | **Codex 版**（如 gpt-5.2-codex） | 90% 寫程式、10% 其他 | 專為編程優化，處理技術任務更精準，但一般對話可能較"工程師口吻" |
  | **通用版**（如 gpt-5.2） | 50% 寫程式、50% 其他 | 寫程式能力依然很強，且更擅長自然對話、創作、翻譯等多元任務 |

  **建議**：
  - 若您希望 AI 助手**既能寫程式、又能聊天討論、處理多元任務**，建議選 **gpt-5.2（通用版）**。
  - 若您**幾乎只用來寫程式、做技術任務**，可選 **gpt-5.2-codex**。
  - **計費相同**，差異主要在對話風格與多元任務的處理品質。
  - 其他選擇：**gpt-5.1-codex-max**（最強性能）、**gpt-5.2-codex-mini**（省額度）。

**若無法連上儀表板（127.0.0.1 拒絕連線）**  
`openclaw dashboard --no-open` 只會印出網址，**不會**自己啟動儀表板伺服器；儀表板是由 **Gateway** 在埠 18789 提供的。請依序檢查：

1. **確認 Gateway 是否有在跑**：執行 `openclaw status` 或 `openclaw health`。若顯示未運行或不健康，請先啟動 Gateway：
   ```bash
   openclaw gateway start
   ```
2. 若您安裝時有選「安裝為背景服務」，Gateway 可能以系統服務方式運行；若未啟動，可執行 `openclaw gateway start` 或依作業系統檢查該服務是否已啟動。
3. 啟動成功後，再執行 `openclaw dashboard --no-open`，並在瀏覽器開啟終端機顯示的網址。

**若儀表板能開但顯示「Health Offline」或「Disconnected from gateway (1006)」**  
代表網頁有開、但 **Gateway 程式沒有在跑或啟動後馬上崩潰**。請依序做：

1. **改在前景啟動 Gateway，看是否有錯誤**：開一個終端機，執行  
   `openclaw gateway`  
   （不要加 `start`，讓它一直跑在同一視窗）。若幾秒內就印出錯誤並結束，把錯誤訊息記下來，常見原因：設定錯誤、缺少 API Key、埠被佔用等。
2. **從另一終端機檢查狀態**：Gateway 前景運行時，再開一個終端機執行 `openclaw status`、`openclaw health`，確認是否顯示為運行中。
3. **查看日誌**：執行 `openclaw logs` 查看是否有崩潰或連線錯誤的訊息。
4. 若前景運行時 Gateway 能穩定不退出，再重新整理儀表板網頁，通常就會從 Offline 變為連線成功。之後若希望背景運行，可再試 `openclaw gateway start`（或依安裝時選擇的系統服務方式啟動）。

**若執行 `openclaw gateway start` 出現「Gateway service not loaded」**  
完整訊息範例：
```
Gateway service not loaded.
Start with: openclaw gateway install
Start with: openclaw gateway
Start with: launchctl bootstrap gui/$UID ~/Library/LaunchAgents/ai.openclaw.gateway.plist
```

**這是什麼意思？**  
「Gateway service not loaded」= **Gateway 沒有安裝為系統背景服務（LaunchAgent）**，所以無法用 `stop`/`start` 指令控制。系統給了您三種解決方式。

**解決方式（選一種）**

| 方式 | 指令 | 說明 | 適合情境 |
|------|------|------|---------|
| **方式一：安裝為背景服務**（推薦） | `openclaw gateway install` | Gateway 會開機自動啟動、在背景運行。安裝後就能用 `openclaw gateway stop`/`start` 控制。 | 長期使用、希望開機自動啟動 |
| **方式二：前景運行**（臨時） | `openclaw gateway` | Gateway 在目前終端機視窗運行，關閉視窗就停止。按 Ctrl+C 可停止。 | 臨時測試、debug 時看 log |
| **方式三：手動載入**（進階） | `launchctl bootstrap gui/$UID ~/Library/LaunchAgents/ai.openclaw.gateway.plist` | 手動載入 LaunchAgent 設定檔（需先確認 `.plist` 檔存在）。 | 進階使用者、手動管理服務 |

**建議做法**：執行 `openclaw gateway install`（一次性安裝，之後就能用 start/stop 控制）。

**為什麼會這樣？**  
在安裝 OpenClaw 時，有個步驟會問您「要不要安裝 Gateway 為背景服務」。如果您當時選「否」或跳過，Gateway 就不會自動安裝為系統服務。現在可以補裝。

---

**若執行 `openclaw gateway install` 後又執行 `launchctl bootstrap` 出現錯誤**  
完整訊息範例：
```
Installed LaunchAgent: /Users/user/Library/LaunchAgents/ai.openclaw.gateway.plist
Logs: /Users/user/.openclaw/logs/gateway.log

Bootstrap failed: 5: Input/output error
Try re-running the command as root for richer errors.
```

**這不是錯誤！原因說明：**  
- ✅ `openclaw gateway install` **已經自動完成安裝 + 啟動**（看到「Installed LaunchAgent」就表示成功）
- ❌ 手動執行 `launchctl bootstrap` 是試圖**重複載入已經在運行的服務**，所以出現 `Bootstrap failed: 5: Input/output error`
- **「Input/output error」** 在這裡的意思：**服務已經載入，無法重複載入**

**您不需要做任何事**：`openclaw gateway install` 已經幫您完成所有步驟，不需要再手動執行 `launchctl bootstrap`。

**確認 Gateway 是否正常運行**：執行 `openclaw status`，應該會看到：
```
│ Gateway service │ LaunchAgent installed · loaded · running (pid xxxxx) │
```
看到 **「loaded · running」** 就表示成功！

---

**Gateway 服務的完整指令參考**

**安裝服務（只需執行一次）**
```bash
openclaw gateway install
```
此指令會自動完成安裝 + 啟動，**不需要再手動執行其他指令**。

**控制 Gateway 服務**（安裝完成後可用）
```bash
openclaw gateway stop     # 停止服務
openclaw gateway start    # 啟動服務
openclaw gateway restart  # 重啟服務（等於 stop + start）
```

**前景運行（不安裝服務，用於測試/debug）**
```bash
openclaw gateway          # 在目前終端運行，按 Ctrl+C 停止
```

**查看狀態與日誌**
```bash
openclaw status           # 查看整體狀態（包含 Gateway 服務狀態）
openclaw health           # 查看健康狀況
openclaw logs             # 查看 Gateway 日誌
openclaw logs --follow    # 即時查看日誌（持續輸出新 log）
```

**卸載服務**（若需要移除）
```bash
openclaw gateway uninstall
```

---

**常見英文訊息中文翻譯**

| 英文訊息 | 中文意思 | 處理方式 |
|----------|----------|---------|
| `Gateway service not loaded` | Gateway 服務未載入（未安裝為背景服務） | 執行 `openclaw gateway install` |
| `LaunchAgent installed · loaded · running` | LaunchAgent 已安裝、已載入、運行中 | 正常，無須處理 |
| `LaunchAgent installed · not loaded` | LaunchAgent 已安裝、但未載入 | 執行 `openclaw gateway start` |
| `LaunchAgent not installed` | LaunchAgent 未安裝 | 執行 `openclaw gateway install` |
| `Bootstrap failed: 5: Input/output error` | 無法載入服務（通常是因為服務已經載入） | 檢查 `openclaw status`，若顯示 running 則無須處理 |
| `Gateway reachable` | Gateway 可連線 | 正常，無須處理 |
| `Gateway unreachable` | Gateway 無法連線 | 執行 `openclaw gateway start` 或前景運行 `openclaw gateway` 看錯誤 |
| `Health Offline` | 儀表板無法連上 Gateway | Gateway 未運行或崩潰，檢查 `openclaw status` 和 `openclaw logs` |
| `Disconnected from gateway (1006)` | 儀表板與 Gateway 的 WebSocket 連線中斷 | 同上，檢查 Gateway 是否運行中 |

---

**若 security audit 出現 CRITICAL：Control UI allows insecure HTTP auth**  
這表示儀表板目前允許「只用 token、走 HTTP」登入，未強制裝置身分或 HTTPS。  
- **若您只在本機使用**（只開 `http://127.0.0.1:18789/`、未對外暴露）：風險可接受，可選擇**不修改**；只要維持 Gateway 只聽 127.0.0.1（`gateway.bind: "loopback"`）即可。  
- **若您想消除此 CRITICAL**：依下列步驟將 `allowInsecureAuth` 改為 `false`。  

**具體步驟：消除 CRITICAL（改 allowInsecureAuth: false）**  
1. 用文字編輯器開啟設定檔：**OpenClaw** 為 `~/.openclaw/config.json`，**ClawdBot** 為 `~/.clawdbot/clawdbot.json`。  
2. 在檔案中找到 `gateway` 區塊，其內有 `controlUi`，再找到 `"allowInsecureAuth": true`。  
3. 將 `true` 改成 `false`（**只改這一個字**，不要動逗號或括號），存檔。  
4. 在終端機執行：`openclaw gateway stop`，再執行：`openclaw gateway start`。  
5. 執行 `openclaw security audit` 或 `openclaw status` 確認 CRITICAL 已消失。  
6. **登入儀表板**：改完後先試著用原本的儀表板網址（`openclaw dashboard --no-open` 會印出含 token 的網址）開一次。若**仍可登入**，表示本機 token 仍被接受，無須額外設定。若**無法登入**，可能需在儀表板中配對**裝置身分**，或改用 HTTPS（如 Tailscale Serve）；若暫時需要恢復，可將 `allowInsecureAuth` 改回 `true` 並重啟 Gateway。

**如何解讀 openclaw status 與 openclaw health**  
執行 `openclaw status` 或 `openclaw health` 後，畫面上會出現 **Overview** 與 **Security audit** 等區塊，可依下表判斷是否正常：

| 項目 | 正常／代表意義 | 若異常可怎麼做 |
|------|----------------|----------------|
| **Gateway** | 顯示 `reachable`、有延遲（如 32ms）表示本機 Gateway 有在跑且連得到。 | 若顯示 unreachable，請先執行 `openclaw gateway start` 或改前景執行 `openclaw gateway` 看錯誤。 |
| **Gateway service** | 顯示 `LaunchAgent installed · loaded · running (pid xxxxx)` 表示背景服務有裝、有載入、有在跑。 | 若為 not installed 或 not running，請執行 `openclaw gateway start` 或依安裝時選擇的方式啟動服務。 |
| **Agents / Sessions** | 有列出 agent 與 session 數量、預設模型等，表示 agent 與對話狀態正常。 | 若完全沒有或報錯，可檢查設定檔與 `openclaw logs`。 |
| **Memory** | 顯示檔案數、chunks、`vector ready` 等，表示記憶功能有在運作。 | 若顯示 **dirty**：表示記憶索引有未寫入的變更，通常會在下次同步時寫入，多數情況不影響使用，可先觀察。 |
| **Channels** | 若有設定 Telegram 等，會顯示 `Enabled: ON`、`State: OK` 等。 | 若 State 非 OK，可檢查該頻道的 token 或設定。 |
| **Node service** | 顯示 `LaunchAgent not installed` 表示**未**將「Node」安裝為系統背景服務。 | 見下方「Node service 是什麼？」。多數本機單機使用**不必**安裝，可略過。 |

**Node service 是什麼？可以控制其他裝置嗎？**  
**Node service** 是 OpenClaw 的**選用**元件，用於「多節點／多裝置」或進階部署情境，**不是**「用這台電腦去遙控其他裝置」的意思。  
- **意思**：Gateway 是「主機」，負責儀表板、頻道（如 Telegram）、agents；**Node** 是選用的「節點」服務，可讓**另一台機器或另一個 process** 以「節點」身分連到這台 Gateway，或讓這台機器以節點身分連到別台 Gateway，用於分散式／多機部署。  
- **LaunchAgent not installed**：表示本機**沒有**把 Node 裝成 macOS 的 LaunchAgent（背景服務）。一般**只在本機跑一個 Gateway** 的使用者不需要 Node 服務；只有當您要「多台機器協作」或進階架構時才需要安裝。  
- **結論**：不是「控制其他裝置」的開關；是「可選的節點服務，多數單機使用不必裝」。若 status 顯示 Node service not installed，維持現狀即可。

**Security audit（1 critical · 2 warn · 1 info）**  
- **1 critical**：多半為「Control UI allows insecure HTTP auth」，處理方式見上一段「若 security audit 出現 CRITICAL」。  
- **2 warn**：常見為「Reverse proxy headers not trusted」「Some configured models are below recommended tiers」。若您**僅本機使用**、未對外暴露 Control UI，可暫不處理；若要對外或更嚴格安全，可依畫面 Fix 建議設定 `gateway.trustedProxies` 或升級模型。  
- **1 info**：為一般說明，可略。

**「Missing API keys」紅字**  
若畫面出現類似「your missing API keys are absolutely judging you」的紅字，表示 **有部分 API key 尚未設定**。  
- 若您**目前只用 Telegram + 某一個模型（如 Gemini）**且對話、儀表板都正常，代表**已用到的 key 已設好**；「missing」多半是指**其他供應商**（如 OpenAI、Anthropic、Brave Search）或選用功能的 key 未填。  
- **要補齊**：執行 `openclaw configure`，依畫面檢查並填入您要使用的服務（例如某個 fallback 模型、網路搜尋）的 API key。  
- **若現有功能都正常**：可先當成提醒，之後有需要再補即可。

**自行修改 Gateway 埠（或相關設定）可以嗎？有什麼限制？**  
- **可以自己改，不需很多前置作業**：只要**改埠號 → 存檔 → 重啟 Gateway** 即可。  
  1. 用文字編輯器開啟設定檔：**OpenClaw** 為 `~/.openclaw/config.json`，**ClawdBot** 為 `~/.clawdbot/clawdbot.json`。  
  2. 在 `gateway` 區塊中找到 `"port": 18789`，把 `18789` 改成您要的數字（例如 `18888`），**只改數字、不要動逗號或括號**，存檔。  
  3. 重啟 Gateway：執行 `openclaw gateway stop`，再執行 `openclaw gateway start`。  
  完成後儀表板網址即為 `http://127.0.0.1:您設的埠/`（例如 `http://127.0.0.1:18888/`）。  
- **注意**：埠號須為 1～65535，且**不要選本機已被佔用的埠**（改前可執行 `lsof -i :埠號` 檢查）；若之後對外開放 Control UI，防火牆需放行該埠。

**改 port 會影響什麼？會影響 AI 龍蝦之間的交流嗎？**  
**不會。** 這個 port（預設 18789）只負責 **儀表板／Control UI** 的連線——也就是**你用瀏覽器開網頁操作介面**時，連到 Gateway 的那個埠。  
**AI 龍蝦之間的交流**（例如 Telegram 收發、sessions、agents、呼叫模型 API、記憶等）都是**同一個 Gateway 程式**在處理，跟「儀表板用哪個埠」無關。改 port 只會讓**你開儀表板的網址**變成 `http://127.0.0.1:新埠/`，**不會**影響 Telegram、對話、模型、記憶或龍蝦彼此之間的運作。

**改 port 後還有什麼其他影響？**  
- **開儀表板**：之後一律用新網址（例如 `http://127.0.0.1:18888/`）；若瀏覽器有舊網址（18789）的書籤，需改成新埠或刪掉重加。  
- **openclaw status / openclaw dashboard**：會讀同一份設定檔，改完重啟後會用新埠，無須額外設定。  
- **若用 SSH 隧道連到遠端儀表板**（例如 `ssh -N -L 18789:127.0.0.1:18789 ...`）：需改成對應新埠，例如 `ssh -N -L 18888:127.0.0.1:18888 ...`，本地瀏覽器開 `http://127.0.0.1:18888/`。  
- **若之後對外開放 Control UI**：防火牆或雲端安全群組需放行**新埠**，不是 18789。  
- **自己寫的腳本或筆記**：若有寫死 18789，記得改成新埠。

---

**如何讓 Mac 長時間運作不睡眠（使用 caffeinate）**  

OpenClaw Gateway 需要**持續運行**才能接收 Telegram 訊息、處理對話、執行排程任務等。若您的 Mac 設定了自動睡眠，可能會導致 Gateway 暫停運作。使用 `caffeinate` 指令可以防止 Mac 睡眠。

**指令說明**
```bash
caffeinate -dimsu -u
```

**各參數意思**

| 參數 | 說明 |
|------|------|
| `-d` | 防止**顯示器**睡眠（Display） |
| `-i` | 防止**系統閒置**睡眠（Idle sleep） |
| `-m` | 防止**磁碟閒置**睡眠（Disk sleep） |
| `-s` | 防止當**電源接上時**系統睡眠（System sleep when on AC power） |
| `-u` | 模擬**使用者活動**（User activity），讓系統認為使用者持續使用中 |

**實際使用方式**

**方式一：單獨運行 caffeinate（推薦用於背景服務）**
```bash
caffeinate -dimsu -u
```
執行後，終端機會顯示 `caffeinate -dimsu -u` 並持續運行，Mac 不會睡眠。  
- **停止**：在該終端機視窗按 `Ctrl+C`
- **適合情境**：您已用 `openclaw gateway install` 安裝 Gateway 為背景服務，只需防止 Mac 睡眠

**方式二：同時啟動 caffeinate 和 Gateway 前景運行**
```bash
caffeinate -dimsu -u openclaw gateway
```
這會同時執行 caffeinate 和 Gateway（前景模式），Gateway 關閉時 caffeinate 也會停止。  
- **停止**：按 `Ctrl+C` 會同時停止 Gateway 和 caffeinate
- **適合情境**：您沒有安裝 Gateway 為背景服務，需用前景方式運行

**方式三：在背景持續運行 caffeinate（開機自動啟動）**  
若希望開機後 Mac 就保持不睡眠，可將 caffeinate 加入登入項目：

① 建立一個簡單的啟動腳本：
```bash
echo '#!/bin/bash
caffeinate -dimsu -u' > ~/keep-awake.sh
chmod +x ~/keep-awake.sh
```

② 打開「系統偏好設定」→「使用者與群組」→「登入項目」，點「+」加入 `~/keep-awake.sh`

或者使用 LaunchAgent（進階方式，較複雜但更可靠）。

**何時使用 caffeinate？**

| 情境 | 是否需要 | 說明 |
|------|---------|------|
| **Mac 設定為永不睡眠** | ❌ 不需要 | 系統已經不會睡眠，無須 caffeinate |
| **Mac 接電源且設定「插電時永不睡眠」** | ❌ 不需要 | 只要保持插電，系統不會睡眠 |
| **Mac 會自動睡眠（電池模式或省電設定）** | ✅ 需要 | 用 caffeinate 防止睡眠 |
| **筆電常蓋上螢幕但想讓 Gateway 繼續跑** | ✅ 需要 | 搭配「合蓋不睡眠」設定或外接顯示器 |
| **臨時外出、想讓 Gateway 持續運行** | ✅ 需要 | 執行 caffeinate 並保持插電 |

**注意事項**

⚠️ **電源**：長時間保持運作時，建議**接上電源**，避免電池耗盡。  
⚠️ **散熱**：Mac 長時間運行會產生熱量，確保通風良好，避免覆蓋散熱孔。  
⚠️ **合蓋模式**：若筆電合上螢幕，預設會睡眠。若需合蓋運行，需搭配外接顯示器或特殊設定（可搜尋「Mac clamshell mode」）。  
⚠️ **節能考量**：若長期需要 24/7 運行，建議考慮使用桌機、Mac mini，或將 Gateway 部署到 VPS（見 `02-OpenClaw-全方位實作與安全指南.md`）。

**如何檢查 caffeinate 是否生效？**

執行後，您可以：
1. 等待原本設定的睡眠時間（例如 10 分鐘），觀察 Mac 是否仍保持喚醒
2. 執行 `pmset -g assertions` 查看當前的電源管理狀態，應該會看到 caffeinate 相關的 assertion

**停止 caffeinate**
- 如果是在終端機前景運行：按 `Ctrl+C`
- 如果找不到是哪個終端機在跑：執行 `pkill caffeinate`（會停止所有 caffeinate 程序）

---

**這跟資安有關嗎？**  
是的，**儀表板／Control UI 的存取方式屬於資安議題**。改 port 頂多是「換個埠、較不顯眼」，**不能取代**真正的防護。資安重點在：① **不把 Control UI 對外暴露**（維持 `gateway.bind: "loopback"`，只聽 127.0.0.1）；② **登入方式**（本機用可接受 `allowInsecureAuth: true`，對外則建議關閉或改用 HTTPS／裝置身分）；③ **若對外開放**：防火牆放行、HTTPS、trusted proxies 等。詳見本文件「若 security audit 出現 CRITICAL」與 **《02-OpenClaw-全方位實作與安全指南》**。

**可以隱藏 port、不讓別人知道，並加密嗎？**  
- **隱藏 port**：無法真正「隱藏」——若服務對外開放，別人掃埠仍可能發現。實務做法：① **只聽 127.0.0.1**（`gateway.bind: "loopback"`）：只有你這台電腦能連，別人從網路連不進來，等同不對外暴露，別人無從得知或連到你的 port。② **改 port**（例如 18888）：可讓針對預設 18789 的自動掃描找不到，但若對外開放，掃全埠仍可能發現；重點還是「不要對外」或透過 **SSH 隧道／VPN** 連回來，不直接暴露 port。  
- **加密**：指的是**連線加密（HTTPS）**，讓瀏覽器與儀表板之間的流量不被竊聽。① **本機 only（127.0.0.1）**：流量只在本機，未經公網，風險較低；若仍要加密，可在本機架 reverse proxy（如 nginx、Caddy）對 127.0.0.1:18789 做 HTTPS（需自簽或 localhost 憑證）。② **對外開放時**：強烈建議用 HTTPS，例如 **Tailscale Serve**、或前面加 **reverse proxy**（nginx／Caddy）做 TLS；官方文件與 **《02-OpenClaw-全方位實作與安全指南》** 有提到 Tailscale Serve 與 trusted proxies。  
- **一句話**：不讓別人知道 → 維持只聽 127.0.0.1、不對外；要加密 → 本機可選 HTTPS reverse proxy，對外則用 HTTPS（Tailscale Serve 或 reverse proxy）。

---

## 三、安全備份與重新安裝（給「要重裝且保留記憶」的人）

**本節是獨立的一整套流程**：只給「已經裝過 OpenClaw、現在要卸載後再裝一次並還原備份」的人使用。  
若您是**第一次安裝**，請只做到「二」為止，**不要**做本節。

下面五個步驟是一條龍做完的：**備份 → 卸載 → 重新安裝 → 還原 → 驗證**。其中「重新安裝」就是回頭用「二」的方法再裝一次，不是要您先裝再刪。

當您需要更新版本、遷移主機或解決嚴重問題時，依下列五個步驟操作即可，不會遺失記憶與知識。

### 步驟一：備份您的「龍蝦」核心資料

在進行任何刪除操作前，**備份是絕對必要的第一步**。OpenClaw 的核心資料（記憶、知識、設定）都儲存在特定目錄中。

#### 1. 需要備份的關鍵檔案與目錄

根據官方文件 [3]，最重要的資料位於 `~/.openclaw/` 目錄下：

| 資料類型 | 檔案/目錄路徑 | 說明 |
| :--- | :--- | :--- |
| **長期記憶** | `~/.openclaw/workspace/MEMORY.md` | 您精心整理的、最重要的核心知識。 |
| **每日記憶** | `~/.openclaw/workspace/memory/` | 以日期命名的每日對話與筆記。 |
| **設定檔** | `~/.openclaw/config.json` | 包含模型、插件等所有設定。 |
| **憑證檔** | `~/.openclaw/credentials/` | 儲存所有 API 金鑰與 Token。**極度敏感！** |
| **對話歷史** | `~/.openclaw/sessions.json` | 正在進行中的對話狀態。 |

#### 2. 執行備份指令

您可以執行以下指令，將所有重要資料備份到 `~/openclaw_backup` 目錄中：

```bash
# 建立一個安全的備份目錄
mkdir -p ~/openclaw_backup

# 備份整個 workspace (包含所有記憶檔案)
cp -r ~/.openclaw/workspace ~/openclaw_backup/

# 備份設定檔、憑證與對話歷史
cp ~/.openclaw/config.json ~/openclaw_backup/
cp -r ~/.openclaw/credentials ~/openclaw_backup/
cp ~/.openclaw/sessions.json ~/openclaw_backup/

echo "備份完成！您的 OpenClaw 核心資料已儲存至 ~/openclaw_backup 目錄。"
```

### 步驟二：徹底解除安裝 OpenClaw

備份完成後，您可以使用官方提供的指令來徹底移除 OpenClaw 及其相關服務。

```bash
# 執行官方的自動化解除安裝指令
# --all 會移除所有相關檔案，包括設定與 workspace
# --yes 會跳過所有確認提示
openclaw uninstall --all --yes
```

此指令會自動停止並移除背景服務、刪除 `~/.openclaw` 目錄、並移除 CLI 工具。

### 步驟三：重新安裝 OpenClaw

現在您的系統已經回到乾淨狀態，請依照本文件第二部分的**本機安裝流程**，使用一鍵腳本或手動方式重新安裝 OpenClaw。

**注意**：在重新安裝後的引導設定過程中，您可以先跳過或填寫任意的臨時資訊，因為我們稍後會用備份來覆蓋它們。

### 步驟四：還原您的「龍蝦」核心資料

安裝完成並至少執行過一次 `openclaw onboard` 後，`~/.openclaw` 目錄會被重新建立。現在，我們可以將之前備份的資料還原回來。

```bash
# 停止正在運行的 OpenClaw 服務，以避免檔案衝突
openclaw gateway stop

# 從備份目錄還原所有資料
cp -r ~/openclaw_backup/workspace ~/.openclaw/
cp ~/openclaw_backup/config.json ~/.openclaw/
cp -r ~/openclaw_backup/credentials ~/.openclaw/
cp ~/openclaw_backup/sessions.json ~/.openclaw/

echo "還原完成！"
```

### 步驟五：啟動並驗證

最後，重新啟動 OpenClaw 服務，並檢查狀態以確認您的記憶與知識是否都已成功還原。

```bash
# 重新啟動服務
openclaw gateway start

# 檢查健康狀況
openclaw health

# 與您的機器人對話，確認它是否記得之前的內容
```

透過以上五個步驟，您就可以在完整保護「龍蝦」記憶的前提下，安全地完成重新安裝，確保您的 AI 夥伴的知識與經驗得以傳承。

---

## 四、參考資料

[1] OpenClaw. (n.d.). *Install*. OpenClaw Docs. Retrieved February 2, 2026, from https://docs.openclaw.ai/install

[2] OpenClaw. (n.d.). *Uninstall*. OpenClaw Docs. Retrieved February 2, 2026, from https://docs.openclaw.ai/install/uninstall

[3] OpenClaw. (n.d.). *Memory*. OpenClaw Docs. Retrieved February 2, 2026, from https://docs.openclaw.ai/concepts/memory
