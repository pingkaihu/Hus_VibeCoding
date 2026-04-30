# Claude Code 事前規劃 Skills 完整指南

> 整理自社群高星數 repository，涵蓋六個主流規劃 skill 的精神分析、比較與選用建議。

---

## 目錄

1. [為什麼需要規劃 Skill？](#why)
2. [Skill 總覽](#overview)
3. [逐一深度分析](#deep-dive)
   - [brainstorming](#brainstorming)
   - [grill-me](#grill-me)
   - [writing-plans](#writing-plans)
   - [planning-with-files](#planning-with-files)
   - [github/spec-kit](#spec-kit)
   - [get-shit-done (GSD)](#gsd)
4. [差異比較表](#comparison)
5. [選用建議與最佳組合](#recommendation)
6. [安裝方式](#installation)

---

## 1. 為什麼需要規劃 Skill？ <a name="why"></a>

社群研究發現，所有主流 AI coding 工作流程最終都收斂到同一個架構模式：

```
Research → Plan → Execute → Review → Ship
```

沒有事前規劃，Claude Code 容易：

- 直接跳入實作，造成後期需求頻繁變更
- 遺漏邊界條件和依賴關係
- 在 context window 填滿後失去對整體架構的掌握
- 生成難以追蹤的程式碼，缺乏可溯源的決策記錄

規劃 Skill 的作用是在「動手前先想清楚」，強制建立人機共識的設計文件，讓後續執行更穩定、可預測。

---

## 2. Skill 總覽 <a name="overview"></a>

| Skill | 來源 | Stars | 定位 | 核心哲學 |
|---|---|---|---|---|
| **brainstorming** | obra/superpowers | ⭐ 172k | 設計探索 → 規格文件 | 任何專案，不論大小，都必須先設計再動手 |
| **grill-me** | mattpocock/skills | ⭐ 32.7k | 壓力測試 | 詰問式追問，找出計劃的邏輯漏洞 |
| **writing-plans** | obra/superpowers | （套件內） | 設計 → 執行任務 | 零容忍 placeholder，細到分鐘級任務 |
| **planning-with-files** | OthmanAdi | 高人氣 | Context 持久化 | Filesystem = 持久記憶，防止 /clear 後遺忘 |
| **spec-kit** | github（官方） | ⭐ 90k | 制度化 SDD | 規格是原始碼，程式碼是建置產物 |
| **GSD** | gsd-build | ⭐ 23k | Solo dev 全流程 | 複雜性在系統裡，不在你的工作流裡 |

---

## 3. 逐一深度分析 <a name="deep-dive"></a>

### brainstorming <a name="brainstorming"></a>

**來源：** [obra/superpowers](https://github.com/obra/superpowers) — ⭐ 172k  
**觸發：** `/superpowers:brainstorm`

#### 核心精神

強制在任何實作動作前完成設計探索與確認。即使是「很簡單」的功能也不例外——skill 明確指出：「簡單的專案正是未經檢視的假設造成最多浪費工作的地方。」

#### 運作流程

```
探索專案現況 → 逐一提問 → 提出 2-3 方案 → 分段呈現設計稿 → 寫入文件 → 呼叫 writing-plans
```

#### 優異之處

- **一次一問**：不用選擇題轟炸，偏好提供選項而非開放式問題
- **YAGNI 執行**：主動從設計中移除不必要的功能
- **2-3 方案比較**：帶推薦理由，避免單一路徑思維
- **分段確認**：每段設計稿都等待用戶確認，避免跑偏
- **自動輸出文件**：儲存至 `docs/plans/YYYY-MM-DD-<topic>-design.md` 並 git commit
- **明確終態**：唯一的下一步是呼叫 `writing-plans`，流程不會迷路

#### 適用情境

- 開始開發任何新功能或專案前
- 需要在多個技術方案間做抉擇
- 想確保 AI 和自己對需求的理解一致

---

### grill-me <a name="grill-me"></a>

**來源：** [mattpocock/skills](https://github.com/mattpocock/skills) — ⭐ 32.7k  
**觸發：** 對話中說 "grill me on this" 或 "grill me on these changes"

#### 核心精神

極簡但高效。整個 skill 只有 10 行，卻實現了一件非常有價值的事：用詰問的方式走完決策樹的每個分支，像技術面試官一樣逐一追問，並主動提供每個問題的推薦答案。

#### 優異之處

- **對每個問題主動給建議答案**：不只問問題，還告訴你它認為的最佳解
- **走完決策樹的每個分支**：解決決策之間的依賴關係
- **自動探索程式碼庫**：如果問題可透過閱讀程式碼回答，自動探索而非詢問
- **極低學習成本**：即插即用，不需要安裝或設定

#### 適用情境

- 你已有初步架構想法，想在執行前找出盲點
- 快速 stress-test 一個設計決策
- 在 PR review 前自我檢視

---

### writing-plans <a name="writing-plans"></a>

**來源：** [obra/superpowers](https://github.com/obra/superpowers)  
**觸發：** 由 `brainstorming` skill 自動呼叫，或 `/superpowers:writing-plans`

#### 核心精神

承接已確認的設計稿，輸出一份「假設執行者什麼背景都不懂」的細粒度任務清單。目標是讓任何有能力的工程師（或 AI subagent）都能無歧義地執行。

#### 優異之處

- **零容忍 placeholder**：明確禁止 TBD、TODO、「implement later」、「add appropriate error handling」等模糊用語
- **任務細到分鐘級**：每個任務 2-5 分鐘，包含精確的檔案路徑
- **TDD 內建**：每個任務都要先寫失敗的測試，再寫實作
- **完整程式碼範例**：不允許只描述要做什麼，必須展示怎麼做
- **自我稽核機制**：完成後對照原始設計稿做 coverage check

#### 適用情境

- brainstorming 完成並獲用戶確認後
- 需要讓 AI subagent 執行複雜功能時
- 希望建立可追蹤、可恢復的開發進度

---

### planning-with-files <a name="planning-with-files"></a>

**來源：** [OthmanAdi/planning-with-files](https://github.com/OthmanAdi/planning-with-files)  
**觸發：** `/planning-with-files:plan` 或 `/planning-with-files:start`

#### 核心精神

實作 Manus AI 的 context engineering 模式（Manus 以此估值 $2B 被 Meta 收購）。

核心洞察：
> "Context Window = RAM（揮發、有限）  
> Filesystem = Disk（持久、無限）  
> → 任何重要的東西都要寫到磁碟"

用三個 markdown 文件作為 AI 的「外部工作記憶」：

| 文件 | 用途 |
|---|---|
| `task_plan.md` | 追蹤階段與進度 |
| `findings.md` | 儲存研究發現 |
| `progress.md` | Session log 與測試結果 |

#### 優異之處

- **Session 自動恢復**：`/clear` 後重新啟動，自動從磁碟讀取上次進度
- **Hook 整合**：在 Claude Code 中，每次 tool use 前自動重新讀取計劃、提醒更新進度
- **跨平台**：支援 40+ coding agents（Claude Code、Codex、Cursor、Gemini CLI 等）
- **多語言支援**：有中文、阿拉伯文、德文、西班牙文變體

#### 適用情境

- 多天、多 session 的長期專案
- Context window 容易爆滿的複雜工作
- 需要在不同時間段恢復開發狀態

---

### github/spec-kit (SDD) <a name="spec-kit"></a>

**來源：** [github/spec-kit](https://github.com/github/spec-kit) — ⭐ 90k（GitHub 官方出品）  
**安裝：** `pip install specify-cli` 或 `uv tool install specify-cli`

#### 核心精神

Spec-Driven Development（SDD）的官方標準化實作。核心思想：

> **規格是原始碼，程式碼是建置產物。**  
> 維護軟體 = 演化規格。Debug = 修正規格或實作計劃。

#### 三道強制關卡

```
/speckit.constitution → /speckit.specify → /speckit.plan → /speckit.tasks → /speckit.implement
```

1. **constitution.md**：鎖定不可妥協的專案原則（如「永遠要寫測試」）
2. **Specify**：需求規格，技術無關
3. **Plan**：技術方案，stack-specific
4. **Tasks**：細粒度任務清單，含平行執行標記 `[P]`
5. **Implement**：執行，每個任務完成前驗證 checklist

#### 優異之處

- **GitHub 官方背書**，有持續維護保障
- **constitution.md**：在規劃前就鎖定團隊原則，避免 AI 擅自妥協
- **規格與技術分離**：specify（需求）和 plan（方案）分開，可獨立演化
- **平行任務標記**：自動識別可並行執行的任務
- **Extension 生態**：社群可擴充，有 Jira 整合、wireframe 生成、adversarial spec review 等

#### 適用情境

- 需要可稽核規格的專案（合規、諮詢交付）
- 多人協作，需要對齊所有人對需求的理解
- 長期大型專案，需要 living documentation

---

### get-shit-done (GSD) <a name="gsd"></a>

**來源：** [gsd-build/get-shit-done](https://github.com/gsd-build/get-shit-done) — ⭐ 23k  
**安裝：** `npx get-shit-done-cc --claude --global`

#### 核心精神

> "我不是 50 人工程公司，不想玩 enterprise theater。  
> 我只是一個想把東西做好的人。"

複雜性封裝在系統裡（29 skills + 12 subagents），用戶只看到幾個簡單命令。

#### 核心命令流程

```
/gsd-new-project → /gsd-discuss-phase N → /gsd-plan-phase N → /gsd-execute-phase N
```

#### 獨特工具（其他 skill 沒有）

**`/gsd-spike`：技術可行性驗證**
- 規劃前先跑 2-5 個小實驗
- 每個實驗有 Given/When/Then 假設
- 結果儲存到 `.planning/spikes/`，未來 session 自動載入

**`/gsd-sketch`：UI 方向探索**
- 生成 2-3 個互動式 HTML mockup 方案
- 確認後結果封裝成 project skill，後續 session 自動載入

#### 優異之處

- **Phase 隔離 context**：每個 phase 在獨立的 context window 執行，防止 context rot
- **品質關卡**：schema drift detection、scope reduction detection（防止 AI 偷偷縮水需求）
- **backlog 機制**：尚未進入規劃的想法可放入 `999.x` backlog 待命
- **輕量化選項**：`--minimal` flag 只安裝 6 個核心 skills，降低 token overhead

#### 適用情境

- 獨立開發者，快速迭代
- 技術方向還不確定，需要先 spike 驗證
- 想要完整的 discuss → plan → execute 閉環但不要 enterprise overhead

---

## 4. 差異比較表 <a name="comparison"></a>

| 維度 | brainstorming | grill-me | writing-plans | planning-with-files | spec-kit | GSD |
|---|---|---|---|---|---|---|
| **核心問題** | 需求不清晰 | 計劃有盲點 | 任務不夠細 | Context 會遺忘 | 規格與實作脫鉤 | 全流程需要整合 |
| **觸發時機** | 任何實作前 | 有初步想法後 | 設計確認後 | 長期專案全程 | 任何實作前 | 從零開始專案 |
| **互動方式** | 逐一提問 | 逐一詰問 + 推薦答案 | 單向輸出 | 背景 hooks | CLI + 斜線命令 | 斜線命令循環 |
| **輸出產物** | design.md + commit | 對話共識 | tasks.md + commit | 3 個 .md 持久文件 | specify/plan/tasks | .planning/ 資料夾 |
| **文件持久化** | ✅ git commit | ❌ 無 | ✅ git commit | ✅ 自動 + session 恢復 | ✅ .speckit/ 資料夾 | ✅ .planning/ |
| **多方案比較** | ✅ 2-3 方案 | ✅ 每題推薦答案 | ❌ 執行設計 | ❌ 不做探索 | ❌ 直接規格化 | ✅ spike 驗證 |
| **TDD 內建** | ❌ | ❌ | ✅ 強制 | ❌ | ✅ tasks 含測試 | ✅ 可選 |
| **跨 session 持久** | ❌ | ❌ | ❌ | ✅ 核心特色 | ✅ 文件版控 | ✅ .planning/ |
| **學習成本** | 低 | 極低 | 低（自動觸發） | 低 | 中（需裝 CLI） | 中（命令較多） |
| **適合規模** | 所有 | 所有 | 中大型 | 中大型 | 中大型 | 所有 |
| **Stars** | 172k | 32.7k | — | 高人氣 | 90k | 23k |

---

## 5. 選用建議與最佳組合 <a name="recommendation"></a>

### 單一 Skill 選用

| 情境 | 推薦 |
|---|---|
| 從零開始規劃功能，需要最完整的方法論 | **brainstorming + writing-plans**（superpowers） |
| 已有初步想法，只需快速壓力測試 5 分鐘 | **grill-me** |
| 長期複雜專案，context 常常爆滿 | **planning-with-files** |
| 需要嚴謹規格，多人協作或需可稽核 | **github/spec-kit** |
| 獨立開發者，技術方向未定，需要 spike 驗證 | **GSD** |

### 最佳組合（推薦）

**輕量日常開發：**
```
grill-me（壓力測試）→ brainstorming（設計文件）→ writing-plans（任務清單）
```

**長期複雜專案：**
```
GSD:gsd-spike（技術驗證）→ spec-kit（規格化）→ writing-plans（細化任務）→ planning-with-files（持久 context）
```

**快速原型驗證：**
```
GSD:gsd-sketch（UI 探索）→ GSD:gsd-spike（技術驗證）→ GSD:gsd-plan-phase（規劃）
```

### 不適合疊加的組合

- `brainstorming` + `spec-kit`：兩者都想主導規劃流程，會有衝突
- `grill-me` 後直接進 `writing-plans`：跳過了設計文件的產出，writing-plans 沒有輸入來源

---

## 6. 安裝方式 <a name="installation"></a>

### obra/superpowers（brainstorming + writing-plans）

```bash
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

使用：`/superpowers:brainstorm`

### mattpocock/skills（grill-me）

```bash
/plugin marketplace add mattpocock/skills
```

使用：對話中說「grill me on this」

### planning-with-files

```bash
/plugin marketplace add OthmanAdi/planning-with-files
/plugin install planning-with-files@planning-with-files
```

或使用 npx：
```bash
npx skills add OthmanAdi/planning-with-files --skill planning-with-files -g
```

使用：`/planning-with-files:start`

### github/spec-kit

```bash
# 需要 Python 3.11+ 和 uv
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
specify init my-project --ai claude
```

使用：`/speckit.constitution`、`/speckit.specify`、`/speckit.plan`

### get-shit-done (GSD)

```bash
npx get-shit-done-cc --claude --global
# 只安裝核心 6 個 skills（降低 token overhead）：
npx get-shit-done-cc --claude --global --minimal
```

使用：`/gsd-new-project`

---

## 延伸資源

- [obra/superpowers README](https://github.com/obra/superpowers)
- [mattpocock/skills](https://github.com/mattpocock/skills)
- [OthmanAdi/planning-with-files](https://github.com/OthmanAdi/planning-with-files)
- [github/spec-kit](https://github.com/github/spec-kit)
- [gsd-build/get-shit-done](https://github.com/gsd-build/get-shit-done)
- [awesome-claude-skills（社群精選）](https://github.com/travisvn/awesome-claude-skills)
- [shanraisshan/claude-code-best-practice](https://github.com/shanraisshan/claude-code-best-practice)

---

*整理於 2026 年 4 月。Skills 生態演化快速，建議定期檢查各 repo 的 CHANGELOG。*
