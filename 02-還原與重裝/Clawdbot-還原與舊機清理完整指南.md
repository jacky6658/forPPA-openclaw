# Clawdbot 還原與舊機清理完整指南

**更新日期**：2026-01-30  
**備份檔**：`clawdbot-backup-20260130.tar.gz`（解壓後為 `clawdbot-backup/`）

---

## 📖 閱讀前必讀：這是什麼、何時用、有必要嗎？

### 這是什麼？
本文件是 **Clawdbot 完整還原與舊機清理指南**，整合了：
- 從備份還原到新電腦的完整步驟
- 舊電腦的 bot 關閉與清除方法
- 日常啟動、Web UI 使用、瀏覽器控制
- 還原後可能出現的路徑與設定問題修正

### 何時需要看這份文件？
| 情境 | 是否需要 | 說明 |
|------|---------|------|
| **換新電腦，要把 Clawdbot 搬過去** | ✅ 需要 | 依「二、新電腦還原步驟」操作 |
| **新電腦裝好後，要關掉舊電腦的 bot** | ✅ 需要 | 依「三、舊電腦關閉與清除」操作 |
| **已還原，但不知道如何啟動或開 Web UI** | ✅ 需要 | 依「四、日常使用」操作 |
| **還原後出現路徑錯誤、token 問題** | ✅ 需要 | 依「四、4.4 路徑與設定修正」排查 |
| **要讓 bot 上網、開瀏覽器執行任務** | ✅ 需要 | 依「五、瀏覽器控制」設定 |
| **第一次安裝 Clawdbot** | ❌ 不需要 | 請先看「01-OpenClaw-本機安裝與安全重裝指南.md」 |
| **只是要重裝但保留記憶（同一台電腦）** | ❌ 不需要 | 請看「01-OpenClaw-本機安裝與安全重裝指南.md」的安全重裝流程 |

### 與其他文件的關係
- **01-OpenClaw-本機安裝與安全重裝指南.md**：第一次安裝或同一台電腦重裝時使用
- **本文件**：換新電腦、要完整還原舊機器的 Clawdbot 時使用
- **Clawdbot-從備份還原與重裝指南.md**：依官方文檔重裝並從備份還原，著重防護層級對應

### 必要嗎？
- **如果您要換新電腦且想保留舊 Clawdbot 的所有記憶與設定** → 必要，這是完整的還原指南
- **如果您只是在同一台電腦重裝** → 不必要，請用 01 文件的安全重裝流程

---

本文件整合：新電腦還原、舊電腦清理、日常啟動與 Web UI、**讓 bot 上網／開瀏覽器**，以及還原後因 NVM 與工作區技能產生的路徑修正。

---

## 目錄（依情境查）

| 我要… | 請看 |
|--------|------|
| 還原備份到新電腦 | **二、新電腦還原步驟** |
| 關掉舊電腦的 bot | **三、舊電腦關閉與清除** |
| 啟動 bot、開 Web UI、查 token | **四、日常使用：啟動與 Web UI** |
| 讓 bot 上網、開瀏覽器、本機執行 | **四、4.2** 與 **五、瀏覽器控制** |
| 還原後報錯（路徑、token、allowTailscale） | **四、4.4 路徑與設定修正** |
| 跟 bot 說「幫我搜尋／到網站做某事」 | **五、5.2 用 agent-browser** |

---

## 一、路徑速查表

| 項目 | 路徑 |
|------|------|
| **備份解壓位置** | `/Users/user/Downloads/備份指南/clawdbot-backup/` |
| **Clawdbot 指令** | `/Users/user/.nvm/versions/node/v24.13.0/bin/clawdbot` |
| **Clawdbot 套件目錄** | `/Users/user/.nvm/versions/node/v24.13.0/lib/node_modules/clawdbot` |
| **還原目標：設定與資料** | `~/.clawdbot/` |
| **還原目標：工作區技能** | `~/clawd/skills/` |

說明：本機使用 **NVM** 管理 Node，Clawdbot 安裝在 NVM 目錄下。查詢：`which clawdbot`、`npm root -g`。

---

## 二、新電腦還原步驟

### 2.1 方式 A：執行還原腳本（建議）

```bash
cd "/Users/user/Downloads/備份指南"
./執行還原.sh
```

若權限不足：`chmod +x "/Users/user/Downloads/備份指南/執行還原.sh"`

### 2.2 方式 B：手動還原

```bash
BACKUP="/Users/user/Downloads/備份指南/clawdbot-backup"
mkdir -p ~/.clawdbot
cp "$BACKUP/clawdbot.json" ~/.clawdbot/
cp -R "$BACKUP/agents" ~/.clawdbot/
cp -R "$BACKUP/memory" ~/.clawdbot/
cp -R "$BACKUP/credentials" ~/.clawdbot/
cp -R "$BACKUP/devices" ~/.clawdbot/
cp -R "$BACKUP/identity" ~/.clawdbot/
cp -R "$BACKUP/cron" ~/.clawdbot/
cp -R "$BACKUP/telegram" ~/.clawdbot/
cp "$BACKUP/exec-approvals.json" ~/.clawdbot/ 2>/dev/null || true
mkdir -p ~/.clawdbot/media
cp -R "$BACKUP/media/inbound" ~/.clawdbot/media/
cp -R "$BACKUP/media/outbound" ~/.clawdbot/media/ 2>/dev/null || true
mkdir -p ~/clawd
cp -R "$BACKUP/skills" ~/clawd/
```

### 2.3 驗證

```bash
clawdbot status
ls -la ~/.clawdbot/clawdbot.json ~/.clawdbot/memory/main.sqlite ~/clawd/skills/
```

### 2.4 可選：目錄權限

若 status 建議：`chmod 700 /Users/user/.clawdbot`

---

## 三、舊電腦關閉與清除

新電腦還原並確認 `clawdbot status` 正常後，在舊電腦依序執行。

| 步驟 | 指令 |
|------|------|
| 停止 daemon | `clawdbot daemon stop` |
| 選做：卸載 Gateway 服務 | `clawdbot daemon uninstall` |
| 解除安裝 | `npm uninstall -g clawdbot` |
| 選做：刪除資料 | `rm -rf ~/.clawdbot ~/clawd` |
| 選做：卸載 ai-pdf-builder | `npm uninstall -g ai-pdf-builder` |

若舊電腦用 NVM，請在當初安裝 clawdbot 的那個 Node 版本下執行。

---

## 四、新電腦日常使用與設定

### 4.1 啟動 bot 與開啟 Web UI

**Daemon（常駐）**

- 啟動：`clawdbot daemon start`
- 停止：`clawdbot daemon stop`
- 重啟：`clawdbot daemon restart`
- 狀態：`clawdbot daemon status`  
（注意：是 `daemon start`，不是 `clawdbot start`。）

**Control UI**

1. 確認 Gateway 在跑：`clawdbot daemon start`
2. 瀏覽器開：**http://127.0.0.1:18789/** 或 **http://localhost:18789/**
3. 登入：若出現「gateway token missing」或「disconnected (1008): unauthorized」，在設定裡貼上 **Gateway token**。  
   Token 位置：`~/.clawdbot/clawdbot.json` → `gateway.auth.token`，或終端機執行：  
   `grep -A2 '"auth"' ~/.clawdbot/clawdbot.json | grep '"token"'`
4. **帶 token 開頁（建議）**：`http://127.0.0.1:18789/?token=你的token`，例如  
   `http://127.0.0.1:18789/?token=3ee28a715052c2f28f9e66add21592d1251cc9ed4771163c`

**常見狀況**

- 貼上 token 仍 disconnected：用「帶 token 的網址」開、貼完按儲存後**整頁重新整理**，或無痕視窗再試；必要時 `clawdbot daemon restart`。
- 「gateway already running」「Port 18789 is already in use」：代表 Gateway 已在背景跑，**不是錯誤**；不要再執行 `clawdbot gateway`，直接開上述網址即可。

**其他指令**

- `clawdbot status --all`、`clawdbot logs --follow`、`clawdbot gateway probe`

---

### 4.2 讓 bot 上網、開瀏覽器與在本機執行

- **上網／瀏覽器**：不要設 `tools.deny=["group:web","browser"]`；目前未設，bot 已可用 web_fetch 與 browser 工具。
- **瀏覽器控制**：兩種方式見 **五、瀏覽器控制**（Chrome 擴充功能 或 agent-browser 技能）。
- **本機執行指令**：由 Exec approvals 控制；目前 `security: "full"` 已允許，詳見 **4.3**。

Security audit 的 CRITICAL（小模型 + web/browser 未關閉）會持續出現，屬刻意取捨；若只在本機自用可接受。

---

### 4.3 Exec approvals（本機執行指令）

設定檔：`~/.clawdbot/exec-approvals.json`。還原後多為 `security: "full"`、`ask: "always"`（每次執行前詢問）。

**調整**：Control UI → **Nodes** → **Exec approvals**

- 維持每次詢問：不改。
- 只對未在清單的指令詢問：**Ask** 改為 `on-miss`，在 **Allowlist** 加路徑（如 `~/clawd/**`）。
- 全部允許且不詢問（風險較高）：**Ask** 改為 `off`。

改完按 Save；或直接編輯 JSON 後 `clawdbot daemon restart`。

---

### 4.4 還原後必做：路徑與設定修正（NVM／還原機）

因本機用 **NVM** 且 **agent-browser 在工作區**，還原後 session 仍指向舊電腦的 `/usr/local/...`，需以下修正。

**步驟 1：讓 clawdbot 套件路徑存在**

```bash
sudo mkdir -p /usr/local/lib/node_modules
sudo ln -sf /Users/user/.nvm/versions/node/v24.13.0/lib/node_modules/clawdbot /usr/local/lib/node_modules/clawdbot
```

可消除 log 中 `ls: /usr/local/lib/node_modules/clawdbot/skills/: No such file or directory`。

**步驟 2：讓 agent-browser 技能可被讀取**

agent-browser 在 `~/clawd/skills/agent-browser/`，但 bot 會讀 `/usr/local/.../clawdbot/skills/agent-browser/`。建立連結：

```bash
sudo ln -sf /Users/user/clawd/skills/agent-browser /usr/local/lib/node_modules/clawdbot/skills/agent-browser
```

完成後：`clawdbot daemon restart`。

**步驟 2b：讓 daemon 找得到 agent-browser 指令**

agent-browser 若用 NVM 安裝，會裝在 `~/.nvm/versions/node/.../bin/agent-browser`；**Clawdbot daemon（LaunchAgent）** 的 PATH 通常不包含 NVM 的 bin，所以 bot 執行時會報 **Unknown command: agent-browser**。

作法：把 agent-browser 連到 daemon 會用到的路徑（例如 `/usr/local/bin`）：

```bash
sudo ln -sf /Users/user/.nvm/versions/node/v24.13.0/bin/agent-browser /usr/local/bin/agent-browser
```

（若你的 Node 版本不是 v24.13.0，請改為實際路徑；可用 `which agent-browser` 查。）  
完成後：`clawdbot daemon restart`。

**步驟 3：gateway.auth.allowTailscale**

若 Control UI 配置頁出現 `gateway.auth.allowTailscale` 錯誤／不明確：在 **閘道** → **身份驗證** 將 **allowTailscale** 設為 **false**（未用 Tailscale 時）。或於 `~/.clawdbot/clawdbot.json` 的 `gateway.auth` 加 `"allowTailscale": false`。

**其他**

- Log 中「Gateway service appears loaded. Stop it first」：可忽略，代表 Gateway 已由 daemon 在跑。
- 新電腦使用者名稱不是 `user`：編輯 `~/.clawdbot/clawdbot.json`，改 `agents.defaults.workspace` 為新電腦路徑（如 `/Users/你的使用者名稱/clawd`）。
- 舊電腦曾改過程式碼（unhandled-rejections、service-env、xelatex 等）：在新電腦對應套件目錄手動再改；路徑見「一、路徑速查表」，《Clawdbot-新電腦還原與安裝指南》第七節有說明。

---

## 五、瀏覽器控制：Chrome 擴充功能 vs agent-browser

### 5.1 兩種方式對照

| 方式 | 用途 | 是否需要 Chrome 擴充功能 |
|------|------|--------------------------|
| **內建 browser 工具** | 控制你「正在用的 Chrome」、開分頁、點擊、填表 | **需要** Clawdbot Browser Relay + 在分頁點「Attach to this tab」 |
| **agent-browser 技能** | 無頭瀏覽器自動化（背景 Chromium），開網頁、搜尋、點擊、填表 | **不需要** 擴充功能；需 `npm install -g agent-browser` 與 **4.4 路徑修正** |

找不到或不想用 Chrome 擴充功能時，用 **agent-browser** 即可做搜尋、開網頁、填表等。

---

### 5.2 用 agent-browser（不需擴充功能）

**前置**：完成 **4.4** 的兩個 symlink、**步驟 2b**（讓 daemon 找得到 `agent-browser` 指令），並 `npm install -g agent-browser`。**還需下載瀏覽器**：agent-browser 使用 Playwright，第一次使用前要下載 Chromium，在終端機執行 `npx playwright install` 或 `agent-browser install`（完成後可再 `clawdbot daemon restart`）。

**怎麼跟 bot 說**

在對話裡**明確說「用 agent-browser 技能」**，例如：

- 「請用 agent-browser 技能幫我到 Google 搜尋 [關鍵字]」
- 「用 agent-browser 打開 https://example.com 然後幫我點 [按鈕]／擷取文字」
- 「用 agent-browser：先開 Google，搜尋 XXX，再點進第一個結果把標題貼給我」

若只說「幫我搜尋」「打開網頁」，bot 可能用內建 browser 工具，就會要求 Chrome 擴充功能 + Attach。

**agent-browser 沒有「search」指令**

搜尋或操作網頁要用**一串指令**，不是單一 `search`：

1. `agent-browser open https://www.google.com`
2. `agent-browser wait --load networkidle`（可選）
3. `agent-browser snapshot -i --json` → 從輸出找搜尋框的 ref（如 @e1）
4. `agent-browser fill @eX "關鍵字"`（@eX 換成實際 ref）
5. `agent-browser press "Enter"` 或 `agent-browser click @eY`（搜尋按鈕 ref）
6. 頁面變了再 `snapshot -i --json`，再做後續操作。

**常用指令**：`open <url>`、`snapshot -i --json`、`fill @eX "文字"`、`type @eX "文字"`、`click @eX`、`press "Enter"`、`wait --load networkidle`、`get text @eX --json`、`get url --json`。  
可把本段流程貼給 bot，請它「照此流程用 agent-browser，不要用 search 指令」。

**若出現「Please run the following command to download new browsers: npx playwright install」**

agent-browser 底層使用 Playwright，需先下載 Chromium 等瀏覽器二進位檔。在**終端機**執行（會下載較久）：

```bash
npx playwright install
```

或使用 agent-browser 自帶的安裝指令：

```bash
agent-browser install
```

完成後再請 bot 用 agent-browser 開網頁；必要時 `clawdbot daemon restart`。

---

### 5.3 用 Chrome 擴充功能（控制「你的 Chrome」）

若要控制**你正在使用的 Chrome**，需安裝 **Clawdbot Browser Relay**，並在要控制的分頁點擴充功能圖示 → **Attach to this tab**。

**常見錯誤**：`Chrome extension relay is running, but no tab is connected`、`tabs: []` → 代表尚未有分頁 Attach，在該分頁點擴充功能並選「Attach to this tab」即可。

**注意**：`browser` 是 Clawdbot **工具**，由 bot 在對話中呼叫，不是你在終端機打的指令；在終端機打 `browser status` 會出現 command not found。

**為什麼舊電腦能「直接在本機打開 Chrome」，還原後不行？**

已比對你的**備份**（`clawdbot-backup/clawdbot.json`）：備份裡**沒有** `"tools": { "deny": ["browser"] }`，所以舊電腦上**內建 browser 工具是開啟的**。

舊電腦上你看到的「在本機打開 Chrome」有兩種可能，備份裡**沒有**紀錄是哪一種（備份不含擴充功能安裝狀態或 Attach 紀錄）：

1. **agent-browser 開「有畫面的」瀏覽器**：用 agent-browser 時若加上 `--headed`，會在本機跳出**看得見的 Chromium 視窗**，看起來就像 Chrome 打開。這種**不需要**裝 Clawdbot Browser Relay，也不需要 Attach。
2. **內建 browser 工具 + Chrome 擴充功能**：若當時有裝 **Clawdbot Browser Relay** 並在 Chrome 分頁點「Attach to this tab」，bot 用 browser 工具就會控制你本機的 Chrome。

若你記得**沒有裝**擴充功能，那舊電腦多半是 **agent-browser 用 `--headed` 開可見視窗**；還原後我們關掉了 browser 工具，但 agent-browser 仍可用，只是預設是無頭模式。若要恢復「本機看到瀏覽器視窗」，可請 bot 用 agent-browser 時加上 `--headed`（例如 `agent-browser --headed open https://...`）。

還原到新電腦後，我們在 `~/.clawdbot/clawdbot.json` 裡**加上了** `"tools": { "deny": ["browser"] }`，把 browser 工具關掉，所以 bot 無法再用內建 browser 控制本機 Chrome，改走 agent-browser。

**若想恢復「在本機打開 Chrome」：**

1. **拿掉 browser 的禁止**：編輯 `~/.clawdbot/clawdbot.json`，刪除 `"tools": { "deny": ["browser"] }` 整段（或只刪掉 `"browser"`，若還有其他 deny 項目）。
2. **在新電腦安裝 Clawdbot Browser Relay**：到 Chrome 線上應用程式商店搜尋並安裝（或從 Clawdbot 文件取得擴充功能連結）。
3. **Attach 分頁**：打開 Chrome → 開一個分頁（或新分頁）→ 點擴充功能圖示 → 選「Attach to this tab」。
4. 執行 `clawdbot daemon restart`。

之後 bot 就可以像舊電腦一樣用 browser 工具控制你這台 Chrome；若沒 Attach 分頁，仍會出現「no tab is connected」，此時 bot 可改用 agent-browser。

---

### 5.4 節點 ID 在哪裡？與「截圖」的兩種方式

**節點 ID 是什麼、在哪裡看**

在 Clawdbot 裡，**節點（Node）** 指的是「執行 bot 的那台裝置」；**節點 ID** 就是該裝置在系統中的識別碼。

- **哪裡看**：打開 **Control UI**（`http://127.0.0.1:18789/`，需輸入 gateway token），左側 **Agent** → **Nodes**（節點），下方 **Nodes** 區塊會列出已連線的執行節點與其 ID。（注意：同頁上方的 **Paired** 是「已配對裝置」的 ID，不是執行節點。）
- 若 **Nodes** 區塊顯示 **「No nodes found.」**：代表目前沒有註冊的執行節點，無法用內建螢幕截圖／錄影；請改用法見下方「若你只是要…截圖」。
- 若你只有一台電腦在跑 daemon 且列表有節點，該節點的 ID 就是你的「節點 ID」。

**為什麼 bot 說「需要 node ID 才能 snapshot」？**

Bot 說「I need a node ID to take a snapshot」時，多半是它想用 Clawdbot 內建的**螢幕截圖／畫面錄製**類工具（例如 `screen_record`），那種工具需要指定「要在哪一個節點（哪台機器）上截圖」，所以會向你要 node ID。

**若你只是要「目前 agent-browser 開的那個網頁」的截圖**

不需要給節點 ID。agent-browser 技能本身就有截圖指令（`agent-browser screenshot page.png`、`agent-browser screenshot --full page.png`），是在**跑 agent-browser 的那台機器**上對瀏覽器視窗截圖，**不需要** Clawdbot 的節點 ID 或 **canvas 工具**。

**Bot 一直說「需要 node ID／canvas tool」時怎麼辦**

Bot 可能誤用 **canvas**（或 screen_record）工具截圖，那種工具才需要 node ID。請**明確告訴 bot 不要用 canvas**，改用 **exec** 執行 agent-browser 的截圖指令，再把產生的圖片檔傳給你。

**可直接貼給 Bot 的訊息（整段複製貼上）：**

請 bot 把截圖**存到你本機找得到的地方**（例如 `~/Downloads`），這樣就算 bot 沒有真的把圖傳到 Telegram，你也可以自己開檔案看：

```
不要用 canvas 工具截圖，我們沒有 node ID。請用 exec 執行這條指令對「目前 agent-browser 已打開的頁面」截圖，存到我的 Downloads：

agent-browser screenshot ~/Downloads/agent-browser-screenshot.png

執行完後請把產生的檔案傳到對話裡給我；若無法上傳，至少告訴我「已存到 ~/Downloads/agent-browser-screenshot.png」，我會自己開來看。
```

若 bot 還是問 node ID，再貼一次上面這段，並強調：「用 exec 跑 agent-browser screenshot，不要用 canvas。」

**Bot 說「已經傳了」但你沒看到圖**

有時 bot 會說「我已經把截圖傳給您了」但對話裡沒有圖。可能是 bot 沒有真的呼叫「上傳媒體到 Telegram」的動作，或權限／路徑問題導致傳送失敗。**補救方式**：請 bot 下次把截圖存到 **`~/Downloads/agent-browser-screenshot.png`**（或你指定的路徑），執行完後你到本機 **Downloads** 資料夾打開該檔即可。若剛才用的是 `/tmp/...`，可再問 bot：「請再截一次，存到 ~/Downloads/agent-browser-screenshot.png，我本機開來看。」

**小結**

| 需求 | 要不要節點 ID | 怎麼做 |
|------|----------------|--------|
| 對 **agent-browser 開的網頁**截圖 | **不需要** | 請 bot 用 agent-browser：`agent-browser screenshot ...` |
| 對 **整台電腦螢幕**截圖／錄影（內建工具） | **需要** | 在 Control UI → **Nodes** 查看你的節點 ID；若顯示 **No nodes found**，代表沒有可用節點，請改用 agent-browser 截圖。 |

---

## 六、檢查清單

**新電腦還原**

- [ ] 已執行還原腳本或手動還原
- [ ] `clawdbot status` 正常，Telegram 為 OK
- [ ] 可選：`chmod 700 ~/.clawdbot`

**還原後路徑與設定（NVM／工作區技能）**

- [ ] 已建立 `/usr/local/lib/node_modules/clawdbot` → NVM clawdbot 的 symlink
- [ ] 已建立 `.../clawdbot/skills/agent-browser` → `~/clawd/skills/agent-browser` 的 symlink
- [ ] 已建立 `/usr/local/bin/agent-browser` → NVM 的 `agent-browser` 的 symlink（讓 daemon 找得到指令）
- [ ] 已設定 `gateway.auth.allowTailscale: false`（若出現錯誤時）
- [ ] 已執行 `clawdbot daemon restart`

**舊電腦清理**

- [ ] 已停止 daemon 或關閉終端機
- [ ] 已 `npm uninstall -g clawdbot`
- [ ] 選做：已刪除 `~/.clawdbot`、`~/clawd`；已卸載 `ai-pdf-builder`

完成後，新電腦為唯一運行 Clawdbot 的環境，bot 可上網、用 agent-browser 開網頁／搜尋，並依設定在本機執行指令。

---

## 七、Log 錯誤整理與排除（依你的 log）

依 `clawdbot-logs-filtered-*.log` 中出現的錯誤，整理如下並依優先順序處理。

| 錯誤類型 | 說明 | 解法 |
|----------|------|------|
| **Unknown command: search** | Bot 呼叫了不存在的 `agent-browser search` | 請 bot 改用流程：`open` → `snapshot -i --json` → `fill` → `press "Enter"`；可把 **5.2** 的流程貼給 bot。 |
| **Chrome extension relay... no tab is connected** | 內建 browser 工具需要 Chrome 擴充功能 + Attach 分頁 | **一勞永逸**：在 `~/.clawdbot/clawdbot.json` 加入 `"tools": { "deny": ["browser"] }`，停用內建 browser 工具，bot 會改用 agent-browser（exec）開網頁，此 log 不再出現。完成後 `clawdbot daemon restart`。或改在對話中說「用 agent-browser 技能」、或安裝擴充功能並 Attach。 |
| **Executable doesn't exist at .../chromium_headless_shell-1208/...** | Playwright／agent-browser 找不到 Chrome Headless Shell | 終端機執行：`npx playwright install chromium`。若曾因 WebKit 失敗，只裝 Chromium 即可。完成後 `clawdbot daemon restart`。 |
| **No API key found for provider "anthropic"** | 某任務（如 slug-generator）要用 Anthropic（Claude）但沒有 API key | 若要使用：執行 `clawdbot agents add main` 依提示設定 Anthropic API key，或把該任務的模型改為你已有 key 的 provider（如 Google）。若不需要 Claude，可忽略或關閉該功能。 |
| **Context overflow: prompt too large for the model** | 輸入內容超過模型的最大 context window | **解法一（推薦）**：改用更大 context 的模型。編輯 `~/.clawdbot/clawdbot.json`，把 `agents.defaults.model.primary` 從 `google/gemini-2.5-flash-lite` 改成 `google/gemini-1.5-pro`（或 `google/gemini-1.5-flash`），然後 `clawdbot daemon restart`。**解法二**：調整 compaction 模式減少歷史訊息。在 `agents.defaults.compaction.mode` 改為 `"aggressive"`（較積極壓縮）或 `"full"`（完整壓縮），然後重啟。 |
| **No available auth profile for google (all in cooldown or unavailable). (rate_limit)** | Google API 觸發 rate limit，所有 profile 都在冷卻中 | **解法一（暫時）**：改用其他 provider 的模型。編輯 `~/.clawdbot/clawdbot.json`，把 `agents.list[0].model` 改成 `google-antigravity/claude-opus-4-5-thinking`（若你有 google-antigravity profile），然後 `clawdbot daemon restart`。**解法二**：等待 cooldown 結束（通常 15-30 分鐘）。查看 `~/.clawdbot/agents/main/agent/auth-profiles.json` 的 `usageStats` 裡 `cooldownUntil` 時間。**解法三**：新增另一個 Google API key profile。執行 `clawdbot agents add main`，選擇 Google provider，輸入新的 API key。 |
| **AbortError: This operation was aborted** | 連線／請求被中斷（常見於關閉或重啟時） | 若只出現在 daemon 重啟或關閉前後，可忽略；若在一般操作中頻繁出現，再查當時操作與網路。 |

**建議處理順序**：  
1. 先完成 **4.4 步驟 2b**（agent-browser 指令在 PATH）與 Playwright Chromium 安裝，讓 agent-browser 能跑。  
2. 在對話中要求 bot 用 agent-browser 流程、不要用 search／內建 browser，減少前兩類錯誤。  
3. 若有使用 Claude／slug-generator 需求，再設定 Anthropic API key 或改模型。
