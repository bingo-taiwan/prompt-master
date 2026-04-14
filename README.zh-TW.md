# Prompt Master（正體中文說明）

> 英文原版：[README.md](./README.md) ｜ 原作者：[nidhinjs/prompt-master](https://github.com/nidhinjs/prompt-master)
> 本文為 `bingo-taiwan` fork 的正體中文說明，方便台灣使用者快速理解。

![](https://s6.imgcdn.dev/YvLVug.png)

一個專為各種 AI 工具撰寫**精準 prompt** 的 Claude Skill。零浪費 token、零浪費 credit，保留完整 context 與記憶。不用再為了同一個答案重複 prompt 四五次。

**支援工具**：Claude、ChatGPT、Gemini、o1/o3、MiniMax、Cursor、Claude Code、GitHub Copilot、Windsurf、Bolt、v0、Lovable、Devin、Perplexity、Midjourney、DALL-E、Stable Diffusion、ComfyUI、Sora、Runway、ElevenLabs、Zapier、Make……幾乎你能想到的 AI 工具都支援。

---

## 🚀 安裝方式

### 推薦方式：Claude.ai 網頁版

1. 把這個 repo 下載成 ZIP
2. 前往 **claude.ai → 側欄 → Customize → Skills → Upload a Skill**

### 或：直接 clone 到 Claude Code 的 skills 目錄（作者標註不建議）

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/bingo-taiwan/prompt-master.git ~/.claude/skills/prompt-master
```

---

## 🔥 它解決什麼問題？

每個 AI 使用者浪費 credit 的方式都一模一樣：

> 寫模糊 prompt → 得到錯誤答案 → 再 prompt → 接近一點 → 再 prompt → 第四次終於對了

這就是三次白費的 API 呼叫。一天 50 個 prompt 累積下來，就是真實的金錢和時間流失。

### 核心洞察

> 「最好的 prompt 不是最長的，而是每個字都承重（load-bearing）。」

大多數「prompt 生成器」讓 prompt 變得更長，但這個 skill 讓 prompt 變得更**銳利**。

---

## 🎯 使用方式

在 Claude 中可以自然地呼叫它：

```
幫我寫一個 Cursor 的 prompt，用來重構我的 auth 模組
```

```
我需要一個 Claude Code 的 prompt 來建 REST API ── 該問我什麼就問
```

```
這是我寫給 GPT-4o 的爛 prompt，幫我改：[貼上 prompt]
```

```
幫我產生一個 Midjourney 的 prompt：夜晚的賽博龐克城市
```

或明確呼叫：

```
/prompt-master

我想請 Claude Code 用 React 和 Supabase 做一個 todo app
```

---

## 運作方式

Prompt Master 對每次請求執行一套結構化 pipeline：

1. **偵測目標工具** — 判斷 prompt 要給哪個 AI 系統，在背景自動路由到對的寫法
2. **抽取 9 個意圖維度** — 任務、輸入、輸出、限制、情境、受眾、記憶、成功條件、範例
3. **問關鍵澄清問題** — 資訊不足時最多問 3 個，絕不囉嗦
4. **自動選框架** — 背景選對的 prompt 架構，使用者看不到框架名稱
5. **只套用安全技術** — 角色指派、Few-shot 範例、XML 結構、錨定反幻覺、記憶區塊
6. **Token 效率稽核** — 砍掉每個不影響結果的字
7. **交付 prompt** — 一個乾淨可複製的區塊，附一行策略說明

---

## 完整範例 1：圖片 prompt

### 使用者輸入
```
幫我寫一個 Midjourney prompt：寫實風格的武士在雨夜中站立
```

### 產生的 prompt
```
lone samurai standing in heavy rain at night, traditional armor,
neon reflections on wet cobblestone street, cinematic lighting,
dramatic shadows, fog, ultra detailed, photorealistic,
shallow depth of field --ar 16:9 --v 6 --style raw

negative: blurry, low quality, watermark, cartoon, anime, extra limbs
```

**🎯 目標工具**：Midjourney · **⚡ 框架**：Visual Descriptor · **💰 Tokens**：輕量（~60）
**💡 策略**：逗號分隔的描述詞優於散文、光線與氛圍在前、鎖定長寬比與版本、負面 prompt 防止風格偏移。

---

## 完整範例 2：寫程式 prompt

### 使用者輸入
```
幫我寫一個 Claude Code prompt：做一個商業儀表板的 landing page，
外觀和質感要完全像 Notion，有流暢動畫和乾淨 UI
```

產生的 prompt 會把「像 Notion 的感覺」翻譯成**具體的 hex 色碼、像素值、字重、動畫時間**，讓 Claude Code 無法猜錯（完整內容見英文原版 README）。

---

## 🤝 支援任何 AI 工具

Prompt Master 內建 **30+ 工具 profile**，每種工具有不同的寫法：

| 類別 | 工具 | 修正重點 |
|------|------|---------|
| 推理 LLM | Claude / ChatGPT / Gemini | 去除贅字、XML 結構、輸出 contract |
| 思考 LLM | o3 / o4-mini / DeepSeek-R1 | **不**加 CoT（思考模型內部已經思考） |
| 本地 LLM | Ollama / Qwen / Llama / Mistral | 問載入哪個模型、較短 prompt、chat template |
| Agentic AI | Claude Code / Devin / Manus | 停止條件、檔案範圍、檢查點輸出 |
| IDE AI | Cursor / Windsurf / Cline / Copilot | 檔案路徑、函式名、禁觸清單 |
| 全端生成 | Bolt / v0 / Lovable / Figma Make | 技術棧版本、不要 scaffold 什麼 |
| 電腦操作 Agent | OpenAI Computer Use / OpenClaw | 螢幕狀態、允許 app、不可逆動作前停止 |
| 搜尋 AI | Perplexity / SearchGPT | 模式區分：搜尋 vs 分析 vs 比較 |
| 圖片 AI | Midjourney / DALL-E / SD / ComfyUI / SeeDream | 描述詞風格、參數、負面 prompt |
| 3D AI | Meshy / Tripo / BlenderGPT / Unity AI | 風格、輸出格式、面數預算、rig 需求 |
| 影片 AI | Sora / Runway / LTX / Kling | 運鏡、時長、剪接風格 |
| 語音 AI | ElevenLabs | 情緒、語速、強調 |
| 自動化 | Zapier / Make / n8n | 觸發 app、動作 app、欄位對應 |

沒在清單上的工具會走 **Universal Fingerprint**：4 個問題就能為任何沒見過的 AI 系統寫 prompt。

---

## 📐 12 個自動選用的 Prompt 模板

| 模板 | 適用情境 |
|------|---------|
| **RTF**（Role, Task, Format） | 快速單次任務 |
| **CO-STAR** | 專業文件、報告、商業寫作 |
| **RISEN** | 複雜多步驟專案 |
| **CRISPE** | 創意寫作、品牌語氣 |
| **Chain of Thought** | 數學、邏輯、除錯、多步驟分析 |
| **Few-Shot** | 需要一致結構化輸出 |
| **File-Scope Template** | Cursor、Windsurf、Copilot 等 code 編輯 AI |
| **ReAct + Stop Conditions** | Claude Code、Devin、AutoGPT 等自主 agent |
| **Visual Descriptor** | Midjourney、DALL-E、SD、Sora 等生成類 |
| **Reference Image Editing** | 編輯既有圖片（自動判斷編輯 vs 生成） |
| **ComfyUI** | 節點式圖片工作流 |
| **Prompt Decompiler** | 拆解、改寫、簡化現有 prompt |

---

## 🛡️ 5 種安全技術（必要時才套用）

Prompt Master **只用效果可靠的技術**。已知會產生幻覺或不穩定輸出的方法（Tree of Thought、Graph of Thought、Universal Self-Consistency、prompt chaining）**明確排除**。

| 技術 | 作用 |
|------|------|
| **角色指派** | 給 AI 一個專家身份，校準深度與用字 |
| **Few-Shot 範例** | 格式一致性比指令更重要時加 2-5 個範例 |
| **XML 結構標籤** | 用 XML 包住區段，Claude 系工具解析最準 |
| **錨定（Grounding）** | 事實性與引用任務加反幻覺規則 |
| **Chain of Thought** | 邏輯任務強制逐步推理（絕不用在 o1/o3） |

---

## 🚫 35 種浪費 credit 的模式（及修正範例）

Prompt Master 會偵測並自動修正 35 種常見的浪費模式，分成 6 類：

- **任務模式（7 種）**：模糊動詞、兩個任務擠一起、沒成功條件、過度放權、情緒化描述……
- **情境模式（6 種）**：假設 AI 記得之前、缺專案背景、忘記技術棧、幻覺邀請、未定義受眾……
- **格式模式（6 種）**：缺輸出格式、隱含長度、沒指派角色、模糊美感形容詞、圖片 AI 沒負面 prompt……
- **範圍模式（6 種）**：沒範圍邊界、沒技術棧限制、Agent 沒停止條件、IDE AI 沒檔案路徑……
- **推理模式（5 種）**：邏輯任務缺 CoT、對思考模型加 CoT（反效果）、沒自我檢查、期待跨 session 記憶……
- **Agentic 模式（5 種）**：沒起始狀態、沒目標狀態、沈默 agent、檔案系統沒鎖、沒人工審核觸發點……

完整對照表見英文原版 README。

---

## 🧠 記憶區塊系統（Memory Block）

當對話有歷史時，Prompt Master 會抽出過去的決策並在 prompt 前面加上記憶區塊，讓 AI 永遠不會違背之前的決定：

```
## Memory (Carry Forward from Previous Context)
- 技術棧：React 18 + TypeScript + Supabase
- Auth 用 JWT 存在 httpOnly cookie，不是 localStorage
- 元件命名：PascalCase，不用 default export
- 設計系統：只用 Tailwind，不寫 custom CSS
- 架構：不用 Redux，只用 context API
```

**這是長對話最大的修正**。大多數浪費的重 prompt 都來自 AI 忘記你之前決定好的事。

---

## ℹ️ 版本歷史

- **1.5.0** — 新增 Agentic AI 與 3D 模型 AI 路由；修正 description 為 189 字元；移除輸出中的 token 估算
- **1.4.0** — 新增參考圖編輯偵測、ComfyUI 支援、Prompt Decompiler 模式；修正 Claude Code 觸發詞
- **1.3.0** — 以 PAC2026 定位結構（30/55/15）重建；靜默路由取代使用者可見的框架選擇
- **1.2.0** — 為注意力架構重構；移除易產生幻覺的技術（ToT、GoT、USC、prompt chaining）
- **1.1.0** — 擴充工具覆蓋、新增記憶區塊系統、35 個 credit-killing 模式
- **1.0.0** — 初版

---

## 📄 授權

MIT：詳見 [LICENSE](./LICENSE)

---

## 🇹🇼 台灣使用者備註

本 fork 由 [bingo-taiwan](https://github.com/bingo-taiwan) 維護，僅新增正體中文說明檔，**未修改原 skill 邏輯**。如果你同時使用 Claude Code、Gemini CLI、Ollama、Midjourney 等多種 AI 工具，這個 skill 可以幫你在不同工具間切換 prompt 寫法時省下大量重工。

若需回報問題或貢獻，請優先前往[原作者 repo](https://github.com/nidhinjs/prompt-master)。
