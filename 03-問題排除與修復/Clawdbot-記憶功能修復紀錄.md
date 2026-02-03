# Clawdbot / OpenClaw 記憶功能修復指南

---

## 📖 閱讀前必讀：這是什麼、何時用、有必要嗎？

### 這是什麼？
本文件是 **記憶功能故障排查與修復指南**，專門處理：
- `openclaw status` 顯示 **Memory unavailable** 的問題
- Bot 回報「記憶搜尋 API 金鑰錯誤」的問題
- Embedding 模型設定錯誤的修復

### 何時需要看這份文件？
| 情境 | 是否需要 | 說明 |
|------|---------|------|
| **執行 `openclaw status` 顯示 Memory unavailable** | ✅ 需要 | 記憶功能有問題，依本文排查 |
| **Bot 回覆說無法存取長期記憶、API 金鑰錯誤** | ✅ 需要 | 記憶搜尋設定有誤，依本文修正 |
| **修改模型設定後出現 "Unrecognized key: embedding"** | ✅ 需要 | 設定方式錯誤，依本文正確設定 |
| **Bot 運作正常、記憶功能正常** | ❌ 不需要 | 無須閱讀，記憶功能正常 |
| **第一次安裝 OpenClaw** | ❌ 不需要 | 先完成安裝與設定，有問題時再回來看 |

### 與其他文件的關係
- **01-OpenClaw-本機安裝與安全重裝指南.md**：安裝與初始設定
- **本文件**：當記憶功能出現故障時的排查與修復
- **Clawdbot-穩定性優化與模型配置指南.md**：模型選擇與 fallback 設定

### 必要嗎？
- **如果您的 Bot 記憶功能正常運作** → 不必要，無須閱讀
- **如果出現上述記憶相關錯誤** → 必要，這是唯一的修復指南

---

若 `clawdbot status`（或 `openclaw status`）顯示 **Memory │ enabled (plugin memory-core) · unavailable**，或 Bot 回報記憶搜尋 API 金鑰錯誤，可依本指南排查。  
**說明**：若您使用 OpenClaw，請將下列 `clawdbot` 改為 `openclaw`，路徑 `~/.clawdbot` 改為 `~/.openclaw`。

---

## 1. 問題現象

- 狀態顯示：**Memory │ enabled (plugin memory-core) · unavailable**
- Bot 回覆：記憶搜尋顯示 API 金鑰錯誤，無法存取長期記憶

---

## 2. 錯誤的設定方式（會報錯）

**不要**用下列方式設定 embedding，會失敗：

```bash
clawdbot config set agents.defaults.model.embedding "google/text-embedding-004"
# Error: agents.defaults.model: Unrecognized key: "embedding"
```

**原因**：`agents.defaults.model` 僅接受對話用 LLM（如 `primary`、`fallbacks`），不包含 `embedding`。

---

## 3. 正確的 Embedding 設定

Embedding 是給**記憶搜尋（memory search）**用的，需寫在 `agents.defaults.memorySearch`：

```bash
# 設定 provider（例如 Gemini）
clawdbot config set agents.defaults.memorySearch.provider "gemini"

# 設定 embedding 模型
clawdbot config set agents.defaults.memorySearch.model "google/text-embedding-004"
```

若版本不支援 `google/text-embedding-004`，可改用：

```bash
clawdbot config set agents.defaults.memorySearch.model "gemini-embedding-001"
```

設定後需 **重啟 gateway**：`clawdbot gateway restart`

---

## 4. 記憶仍 unavailable：API 金鑰問題

設定完 provider/model 後若仍顯示 unavailable 或 API 金鑰錯誤，多半是**記憶搜尋使用的 Gemini Embedding API 需要 Google API 金鑰**。僅有 OAuth（如 google-antigravity）不一定能提供 embedding，需有一組 **API Key** 型態的 Google 金鑰。

---

## 5. auth-profiles.json 位置與用途

- **路徑**：`~/.clawdbot/agents/main/agent/auth-profiles.json`（OpenClaw 則為 `~/.openclaw/` 對應結構）
- Bot 由此讀取各 provider 認證。記憶搜尋需要一組 **google、type: api_key** 的 profile（例如 `google:default`）。

---

## 6. 解法：加入 Google API 金鑰

使用 **Google AI Studio 等提供的免費 API 金鑰**即可，僅供 **embedding（記憶搜尋）** 使用，不影響對話模型（GPT / Claude 等）。

在 `auth-profiles.json` 的 `profiles` 中新增或修正：

```json
"google:default": {
  "type": "api_key",
  "provider": "google",
  "key": "您的_Google_API_金鑰"
}
```

在 `lastGood` 中確保有：

```json
"google": "google:default"
```

存檔後執行：`clawdbot gateway restart`

---

## 7. 修復成功的表現

重啟後 `clawdbot status` 的 Memory 行會顯示類似：

```
Memory │ N files · M chunks · ... · vector ready · fts ready · cache on (...)
```

表示已索引記憶檔、向量與全文搜尋就緒，長期記憶可正常使用。

---

## 8. 若出現「gemini batch failed」404 錯誤

日誌出現：

```
[memory] embeddings: gemini batch failed; ... asyncBatchEmbedContent not available ...
Disable remote.batch.enabled or switch providers.
```

**原因**：部分 API 或 embedding 模型不支援 Gemini 的非同步批次介面。

**解法**：關閉記憶搜尋的遠端批次，改為一般 embedding。

在設定檔（如 `~/.clawdbot/clawdbot.json`）的 `agents.defaults.memorySearch` 中加上：

```json
"remote": {
  "batch": {
    "enabled": false
  }
}
```

或執行：

```bash
clawdbot config set agents.defaults.memorySearch.remote.batch.enabled false
```

設定後執行 `clawdbot gateway restart`。

---

## 9. 日後換機或重裝可參考

1. **Embedding**：使用 `agents.defaults.memorySearch.provider` 與 `agents.defaults.memorySearch.model`，勿用 `agents.defaults.model.embedding`。
2. **金鑰**：在 `auth-profiles.json` 保留或還原 `google:default`（api_key），或改設環境變數 `GEMINI_API_KEY`。
3. **重啟**：任何 config 或 auth 變更後執行 `clawdbot gateway restart`。
