# Superpowers (obra/superpowers) 完整介紹

> 一套為 AI coding agent 設計的完整軟體開發方法論框架
> GitHub: https://github.com/obra/superpowers

---

## 目錄

1. [簡介](#1-簡介)
2. [架構](#2-架構)
3. [Sub-agent 設計](#3-sub-agent-設計)
4. [使用情境](#4-使用情境)
5. [學習資源](#5-學習資源)
6. [學習建議](#6-學習建議)

---

## 1. 簡介

### 什麼是 Superpowers？

Superpowers 是一套完整的軟體開發工作流程框架，建立在一組可組合的「Skills」和初始指令之上，讓你的 coding agent 自動遵循這些流程。它不是讓 AI 直接跳進去寫程式，而是先強制走一套有紀律的開發流程。

由 Jesse Vincent（obra）開發，MIT 授權，免費開源。2026 年 1 月 15 日正式進入 Anthropic Claude Code 官方 Plugin Marketplace。

### 核心哲學

| 原則 | 說明 |
|------|------|
| **Test-Driven Development** | 永遠先寫測試，再寫實作 |
| **系統性 > 臨時應對** | 有可重複的流程，勝過每次猜測 |
| **降低複雜度（YAGNI）** | 以簡單為第一目標，不做 spec 沒要求的東西 |
| **以證據驗收** | 宣告完成前先驗證，不只是聲稱 |

### 支援平台

- Claude Code（官方 Marketplace）
- Cursor
- OpenAI Codex
- OpenCode
- GitHub Copilot CLI
- Gemini CLI

### 安裝方式

**Claude Code：**
```bash
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

**Cursor（Agent chat）：**
```
/add-plugin superpowers
```

**Gemini CLI：**
```bash
gemini extensions install https://github.com/obra/superpowers
```

---

## 2. 架構

### Skills 系統

整個框架的核心單位是 **Skill**——每個 Skill 是一個 `SKILL.md` Markdown 文件，定義了某個工作情境下 agent 應該怎麼行動。

**Skills 的關鍵特性：**

- **自動觸發**：agent 在每個任務前會主動查看哪些 skill 適用，不需要手動呼叫
- **強制工作流**：不是「建議」，是 mandatory workflows
- **1% 原則**：只要有 1% 的機率某個 skill 適用，agent 就必須使用它
- **可擴充**：可用 `writing-skills` skill 撰寫自己的 custom skill

### Skills 分類

```
skills/
├── brainstorming/              # 需求釐清、設計文件
├── using-git-worktrees/        # 隔離工作分支
├── writing-plans/              # 任務拆分計畫
├── subagent-driven-development/ # Sub-agent 執行引擎（核心）
├── executing-plans/            # 無 sub-agent 支援時的替代方案
├── test-driven-development/    # TDD 強制循環
├── systematic-debugging/       # 系統性除錯
├── verification-before-completion/ # 完成前驗證
├── requesting-code-review/     # 發起 code review
├── receiving-code-review/      # 處理 review 意見
├── finishing-a-development-branch/ # 結案分支
├── dispatching-parallel-agents/ # 並行 agent 派發
└── writing-skills/             # 撰寫新 skill 的方法論
```

### 七步驟開發流程

```
1. Brainstorming
   └─ 蘇格拉底式對話，釐清需求，產出設計文件

2. Git Worktree 建立
   └─ 隔離工作環境在新 branch，確認乾淨測試基線

3. Writing Plans
   └─ 拆分為 2–5 分鐘小任務，含精確檔案路徑與驗證步驟

4. Subagent-Driven Development ★
   └─ 每個任務派一個 fresh sub-agent，兩階段 review

5. Test-Driven Development
   └─ RED → GREEN → REFACTOR 強制循環

6. Code Review
   └─ 按嚴重性分級，critical 會阻擋後續進度

7. Finish Branch
   └─ merge / PR / 保留 / 丟棄 四選一，清理 worktree
```

### 指令優先順序

```
使用者明確指令（CLAUDE.md / 直接要求）   ← 最高優先
    ↓
Superpowers Skills
    ↓
預設系統行為                             ← 最低優先
```

---

## 3. Sub-agent 設計

### 核心設計原則

> **Fresh sub-agent per task + Two-stage review = 高品質、快速迭代**

傳統 agent 在長時間執行後，context window 會累積歷史包袱，導致逐漸偏離原始需求（context pollution）。Superpowers 的解法是：

- **Controller agent** 負責協調、管理任務隊列
- **Fresh sub-agent** 每個任務從零開始，只接收精確構建的必要 context
- Sub-agent 絕對不繼承前一個任務的 session 歷史

### 兩階段 Review 流程

每個任務執行完後，強制經過兩個獨立的 reviewer：

#### Stage 1：Spec Compliance Review（規格符合審查）

| 檢查重點 | 說明 |
|----------|------|
| 需求是否完整實作？ | 有無漏掉的 requirements |
| 是否過度實作？ | 有無做了 spec 沒要求的東西（YAGNI） |
| 審查方式 | 直接讀程式碼，不信 implementer 的自我報告 |

> 只有 Stage 1 通過，才會啟動 Stage 2。

#### Stage 2：Code Quality Review（程式碼品質審查）

| 檢查重點 | 說明 |
|----------|------|
| 程式碼風格 | 是否遵守既有 pattern 和慣例 |
| Error handling | 是否適當處理錯誤 |
| Type safety | 防禦性程式設計 |
| 命名與可維護性 | 組織結構、命名慣例 |
| 檔案大小 | 是否造成新檔案過大或既有檔案顯著膨脹 |

#### 完整迴圈

```
Controller
  └─ 派發 Implementer（fresh instance）
       └─ 實作完成，回報狀態訊號
            ├─ DONE
            ├─ DONE_WITH_CONCERNS
            ├─ NEEDS_CONTEXT  → 回問 Controller
            └─ BLOCKED        → 升級處理

  └─ 派發 Spec Reviewer
       ├─ ❌ 不符合 → Implementer 修正 → 同 reviewer 再審
       └─ ✅ 符合

  └─ 派發 Code Quality Reviewer
       ├─ ❌ 品質不足 → Implementer 修正 → 同 reviewer 再審
       └─ ✅ 通過

  └─ TodoWrite 標記完成 → 下一個任務 / Final Review
```

### 模型選擇建議

| 任務類型 | 建議模型 |
|----------|----------|
| 機械性 1–2 個檔案任務 | 便宜模型（省成本） |
| 多檔案整合任務 | 標準模型 |
| 架構設計、Review | 最強模型 |

### Prompt 文件結構

```
skills/subagent-driven-development/
├── SKILL.md                     # 主要流程定義
├── implementer-prompt.md        # Implementer sub-agent 的指令模板
├── spec-reviewer-prompt.md      # Spec compliance reviewer 的指令模板
└── code-quality-reviewer-prompt.md  # Code quality reviewer 的指令模板
```

### 並行 Sub-agent（進階）

使用 `dispatching-parallel-agents` skill，可以同時派多個 sub-agent 分別處理獨立模組，例如同時開發 I/O 層和演算法核心，大幅縮短整體開發時間。

---

## 4. 使用情境

### 情境 A：從零開始一個新功能

最典型的使用方式，完整走完七步驟流程。

```
你：「我想做一個 CSV 批次轉換工具，支援自定義欄位映射」

Superpowers 流程：
1. brainstorming  → 反問：需要支援哪些編碼？輸出格式？錯誤處理方式？
2. worktree       → 建立 feature/csv-converter branch
3. writing-plans  → 拆成 5 個任務，每個 2–5 分鐘
4. sub-agents     → 逐一派發執行 + 兩階段 review
5. TDD            → 每個任務先寫失敗測試再實作
6. code review    → 最終整體審查
7. finish branch  → 確認後 merge
```

### 情境 B：系統性除錯

當遇到難以重現的 bug，`systematic-debugging` skill 會強制 agent：

1. 先**複現** bug，而不是直接猜測原因
2. 找到**根本原因**，而不是修表面症狀
3. **驗證修復**真的解決了問題，而不只是症狀消失

### 情境 C：重構既有程式碼

隔離的 git worktree + 小計畫 + 測試驗證，讓高風險的重構變得可控：
- 每個重構步驟都有測試保障
- 可以隨時回到原始狀態
- 兩階段 review 確保重構沒有引入新問題

### 情境 D：並行開發不同模組

當功能可以拆分為獨立模組（如前端 UI、後端 API、資料庫 schema），使用 `dispatching-parallel-agents` 同時派發多個 sub-agent 處理，所有 agent 完成後再整合。

### 情境 E：撰寫你自己的 Skill

如果你有重複性的工作流程（例如每次都要做特定格式的 code review、或特定領域的驗證邏輯），可以用 `writing-skills` skill 把它封裝成自己的 `SKILL.md`，之後 agent 會自動套用。

---

## 5. 學習資源

### 官方文件

| 資源 | 說明 | 連結 |
|------|------|------|
| GitHub README | 安裝指南、所有 skills 清單 | https://github.com/obra/superpowers |
| DeepWiki | 從 source code 自動產生的完整架構文件 | https://deepwiki.com/obra/superpowers |

### 作者第一手文章

| 文章 | 說明 |
|------|------|
| Superpowers: How I'm using coding agents in October 2025 | 作者親自說明設計哲學，Skills 概念的起源 |
| 連結：https://blog.fsck.com/2025/10/09/superpowers/ | |

### YouTube 影片教學

| 影片 | 說明 |
|------|------|
| Superpowers: NEW Spec Toolkit Ends Vibe Coding! (Full Tutorial) | 完整操作示範，spec-driven 開發流程 |
| https://www.youtube.com/watch?v=7cOAayWzYDY | 2026 年 3 月 |
| Claude Code + SUPERPOWERS = The End of Vibe Coding? (Full Tutorial) | 著重 TDD 強制執行的實際演示 |
| https://www.youtube.com/watch?v=TX91PdBn_IA | 近 4 週 |
| Superpowers + Claude Code, Antigravity, OpenCode | 宏觀介紹 + 多平台整合 |
| https://www.youtube.com/watch?v=j79iwj0p66k | 近 4 週 |

### 文字教學文章

| 文章 | 說明 |
|------|------|
| Superpowers Tutorial Series（11 篇系列） | 從安裝到每個 skill 的完整系列，附常見問題排解 |
| https://www.ququ123.top/en/2026/02/superpowers-setup-guide/ | |
| Superpowers for Claude Code: Complete Guide 2026 | 含 Notion clone 完整實作範例 |
| https://www.pasqualepillitteri.it/en/news/215/superpowers-claude-code-complete-guide | |
| Medium：Claude Code Plugin for an Agentic Software Development Workflow | 概念介紹與安裝說明 |
| https://new2026.medium.com/superpowers-obra-superpowers-claude-code-plugin-for-an-agentic-software-development-workflow-1e7bdffeb065 | |

### 社群

| 管道 | 說明 |
|------|------|
| Discord | 最活躍的支援管道，分享 custom skills、問安裝問題 |
| https://discord.gg/35wsABTejz | |
| GitHub Issues | 回報 bug、提功能需求 |
| https://github.com/obra/superpowers/issues | |

---

## 6. 學習建議

### 建議學習路線

```
Step 1：理解設計哲學
  → 閱讀作者的原始文章（blog.fsck.com）
  → 了解「為什麼需要這套流程」，而不只是「怎麼用」

Step 2：安裝並跑通第一個任務
  → 選擇你熟悉的平台（推薦 Claude Code）
  → 用一個小專案完整走一遍七步驟流程
  → 不要跳過 brainstorming，這是最容易被忽略但最重要的一步

Step 3：深入 sub-agent 流程
  → 觀察 fresh sub-agent 和兩階段 review 的實際效果
  → 閱讀 DeepWiki 的 subagent-driven-development 章節

Step 4：撰寫自己的 Skill
  → 找出你的重複性工作流程
  → 用 writing-skills 把它封裝成 SKILL.md
  → 這是從「使用工具」到「擴充工具」的關鍵一步
```

### 常見新手誤區

| 誤區 | 正確做法 |
|------|----------|
| 安裝後直接讓 agent 寫程式 | 先讓 brainstorming skill 跑完，確認需求再開始 |
| 跳過 writing-plans 直接執行 | 計畫是 sub-agent 的錨點，不能省略 |
| 以為 skills 只是建議 | Skills 是強制工作流，agent 必須遵守 |
| 只用在大型專案 | 小功能也值得用，習慣流程後開銷很低 |

### 進階：自訂工作流程

Superpowers 完全允許你覆蓋預設行為：

```markdown
# 在 CLAUDE.md 中加入
# 這會讓你的指令優先於 skills 的設定

superpowers: 跳過 brainstorming，直接從 writing-plans 開始
```

或是在專案根目錄建立 `CLAUDE.md`，指定哪些 skills 不適用於這個專案。

---

*文件整理自 GitHub obra/superpowers、DeepWiki、作者部落格及社群教學資源。*
*整理日期：2026 年 4 月*
