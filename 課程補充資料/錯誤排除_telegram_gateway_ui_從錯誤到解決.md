# 錯誤排除：Telegram 設定＋Gateway 啟動＋UI 連線（從錯誤到解決）

> 適用版本：OpenClaw 2026.2.x（你拍攝當下版本為 2026.2.2-3）
> 
> 目的：把「我剛剛遇到的錯誤」整理成新手照做就能解掉的流程。

---

## 0) 先說結論（新手最常卡的 2 個點）
1) **Telegram 設定路徑改版**：`telegram.*` 改成 `channels.telegram.*`
2) **Gateway 會被擋住**：若 `gateway.mode` 沒設，UI 會連不上（拒絕連線）

---

## 1) 錯誤一：設定 telegram.botToken 失敗（Config validation failed）
### 現象
在終端機輸入：
```bash
openclaw config set telegram.botToken "你的Token"
```
會看到類似訊息：
- `Config validation failed: telegram: telegram config moved to channels.telegram ...`

### 原因
新版 OpenClaw 已把 Telegram 設定搬到：
- `channels.telegram`

### 解法（正確指令）
請改用：
```bash
openclaw config set channels.telegram.botToken "你的Token"
```

> 提醒：Token 像密碼一樣，不要貼到群組或公開地方。

---

## 2) 錯誤二：UI 顯示「拒絕連線」（ERR_CONNECTION_REFUSED）
### 現象
瀏覽器打開：
- `http://127.0.0.1:18789/`
顯示「無法連上這個網站 / 拒絕連線」。

### 可能原因（最常見）
Gateway 其實沒有真正啟動成功（看起來 service loaded，但 port 沒在 listen）。
常見提示：
- `Gateway port 18789 is not listening`
- `Service is loaded but not running (likely exited immediately)`

---

## 3) 錯誤三：Gateway start blocked（gateway.mode=local 未設定）
### 現象
在 log 或 status 裡重複看到：
- `Gateway start blocked: set gateway.mode=local (current: unset) ...`

### 原因
新版 OpenClaw 需要你明確指定 gateway 的運作模式；若未設定，會阻擋啟動。

### 解法（最快）
**步驟 1：設定 gateway.mode=local**
```bash
openclaw config set gateway.mode local
```

**步驟 2：重啟 gateway**
```bash
openclaw gateway restart
```

**步驟 3：確認 gateway 有在 listen**
```bash
openclaw gateway status
```
看到類似：
- `Listening: 127.0.0.1:18789`
就代表成功。

**步驟 4：打開 UI**
```bash
openclaw dashboard
```

---

## 4) `doctor --fix` 是什麼？（為什麼它會幫你做很多事）
### 你會看到的訊息
- `Restarted LaunchAgent: gui/501/ai.openclaw.gateway`
- 以及一些幽默標語（不是錯誤）

### 它做了什麼
- 執行環境檢查（Doctor）
- 自動補齊/修正部分設定
- 安裝或重啟 macOS 的 LaunchAgent（讓 gateway 能在背景運作）

### 建議用法
當你遇到「gateway 啟動不順」時，先跑：
```bash
openclaw doctor --fix
```
再跑：
```bash
openclaw gateway restart
```

> 但若仍卡在 `gateway.mode=local unset`，就用上一節的手動設定最快。

---

## 5) 重要觀念：UI 為什麼在另一台電腦會連不上？
如果你在另一台電腦打 `127.0.0.1:18789`，一定連不上。
- `127.0.0.1` 代表「**本機**」
- OpenClaw 預設是 loopback-only（只允許本機連線）

**結論：Control UI 只能在「跑 gateway 的那台電腦」打開。**

---

## 6) 快速排查清單（你只要照順序跑）
1) 設定 Telegram Token（新版路徑）
```bash
openclaw config set channels.telegram.botToken "你的Token"
```
2) 設定 gateway mode
```bash
openclaw config set gateway.mode local
```
3) 重啟 gateway
```bash
openclaw gateway restart
```
4) 開 UI
```bash
openclaw dashboard
```

---

## 7) 影片口白建議（一句話講清楚）
- 「如果 UI 顯示拒絕連線，通常是 gateway 沒真的跑起來。」
- 「新版 OpenClaw 把 telegram 設定搬到 `channels.telegram`，另外 gateway 需要設定 `gateway.mode=local` 才會啟動。」
