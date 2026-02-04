# Vibe Coding 學習重點指南

這是一份專為 Vibe Coding（透過 AI 輔助進行高效、流暢的程式開發）整理的學習路徑與重點摘要。Vibe Coding 強調開發者與 AI Agent 之間的協作默契，將繁瑣的編碼工作交給 AI，人類專注於架構、邏輯與創意。

---

## 1. 核心前後端工具 (Core Stack)

AI 能寫出很好的程式碼，但你必須看得懂並能整合它們。

### 技術選擇與替代方案

| 項目 | 類型 | 替代方案 | 為什麼重要？ |
| :--- | :--- | :--- | :--- |
| **JSON** | 不可取代 | 無 | AI 與程式交換資料的通用語言 |
| **API 觀念** | 不可取代 | 無 | 理解 Request/Response 才能指揮 AI 串接功能 |
| **Git / GitHub** | 不可取代 | GitLab, Bitbucket | 版本控制是 AI 開發的「防波堤」 |
| **Terminal / CLI** | 極推薦 | 無 | 看不懂 CLI 就無法執行 AI 給的指令 |
| **Markdown** | 極推薦 | 無 | 與 AI 溝通的主要格式 |
| **環境變數 (.env)** | 極推薦 | 無 | 資安基礎，API Keys 不能 Hardcode |
| **TypeScript** | 極推薦 | JavaScript | 型別定義能大幅減少 AI 幻覺 |
| **Next.js / React** | 可替代 | Vue, Svelte | AI 訓練資料最豐富的框架 |
| **Node.js** | 可替代 | Python, Go | AI 對主流語言支援度都很高 |
| **TailwindCSS** | 可替代 | CSS Modules, Sass | Utility-first 最適合 AI 生成 |

### 進階選修

| 項目 | 說明 |
| :--- | :--- |
| **Database** | CRUD 與關聯式資料庫概念，推薦 Prisma / Drizzle |
| **Docker** | 容器化技術，確保環境一致性 |
| **OAuth / JWT** | 認證機制，登入系統必修 |

> [!TIP]
> 保持 **TypeScript + TailwindCSS** 組合能極大化 AI 產出效率。

---

## 2. 網站部署工具 (Deployment)

### 前端/全端部署平台

| 平台 | 類型 | 最適合 | 免費方案 |
| :--- | :--- | :--- | :--- |
| **Vercel** | 首選 | Next.js 全端專案 | ✅ 慷慨 |
| **GitHub Pages** | 可替代 | 純靜態網站、作品集 | ✅ 完全免費 |
| **Netlify** | 可替代 | 靜態網站 + Serverless | ✅ 慷慨 |
| **Cloudflare Pages** | 可替代 | 需要全球 CDN 加速 | ✅ 非常慷慨 |
| **Railway** | 可替代 | 持久後端伺服器 | ⚠️ 有限 |

### 後端服務 (BaaS)

| 平台 | 類型 | 功能 | 免費方案 |
| :--- | :--- | :--- | :--- |
| **Supabase** | 推薦 | PostgreSQL + Auth + Storage | ✅ 慷慨 |
| **Firebase** | 可替代 | NoSQL + Auth + Hosting | ✅ 有限 |
| **Neon** | 可替代 | PostgreSQL Serverless | ✅ 慷慨 |

> [!TIP]
> 推薦組合：**Vercel + Supabase**，零伺服器管理的全端架構。

---

## 3. Vibe Coding 專用工具 (Tools)

| 類別 | 工具 | 類型 | 說明 | 費用 |
| :--- | :--- | :--- | :--- | :--- |
| IDE | **Cursor** | 首選 | VS Code 深度 AI 整合 | Free / $20/月 |
| IDE | **Antigravity** | Agent 型 | 完全對話式開發，可執行指令 | 依模型計費 |
| IDE | Windsurf | 可替代 | Cascade Agent 功能 | Free / $15/月 |
| UI 生成 | **v0.dev** | 推薦 | React + Tailwind UI 生成 | Free / Pro |
| UI 生成 | Bolt.new | 可替代 | 全端 MVP 專案生成 | Free / Pro |

> [!TIP]
> 推薦組合：**Cursor + Antigravity + v0.dev**，覆蓋完整工作流。

---

## 4. Vibe Coding 技巧 (Skills)

### Prompt Engineering

| 技巧 | 說明 |
| :--- | :--- |
| **明確性** | 不說「修好它」，要說「這個按鈕在手機版破版，請將 padding 改為 12px」 |
| **上下文** | 使用 `@Files` 或 `@Docs` 將相關檔案餵給 AI |
| **角色扮演** | 告訴 AI「你是資深 React 專家，請重構這段 Code」 |

### 迭代優化

不要期待一次完美。先求有，再求好。與 AI 像同事對話：「這個版本不錯，但 header 顏色太深了，請調整...」

### 擴充 AI 能力

| 工具/方法 | 設定方式 | 用途 |
| :--- | :--- | :--- |
| **Cursor `.cursorrules`** | 專案根目錄建立檔案 | 強制 AI 遵守 Coding Style |
| **Antigravity Skills** | 建立 `skills/SKILL.md` | 讓 AI 擁有特定領域能力 |
| **Context Optimization** | 整理專案目錄結構 | 讓 AI 快速「讀懂」專案 |

> [!TIP]
> `.cursorrules` 範例：「所有 React 元件必須使用 Functional Components。」

---

## 5. AI Model 選擇 (Models)

### 模型能力比較

| 模型 | 公司 | Coding | Context | 速度 | 最適場景 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Claude 3.5 Sonnet** | Anthropic | ⭐⭐⭐⭐⭐ | 200K | 中 | 首選，品質最高 |
| **Claude 3.5 Haiku** | Anthropic | ⭐⭐⭐⭐ | 200K | 快 | 快速迭代、省錢 |
| **Gemini 2.0 Flash** | Google | ⭐⭐⭐⭐ | 1M+ | 極快 | 閱讀大型 Codebase |
| **GPT-4o** | OpenAI | ⭐⭐⭐⭐ | 128K | 快 | 綜合能力、邏輯推理 |
| **DeepSeek V3** | DeepSeek | ⭐⭐⭐⭐ | 128K | 中 | 高性價比、開源 |

### 場景推薦

| 場景 | 推薦模型 |
| :--- | :--- |
| 日常開發 | Claude 3.5 Sonnet |
| 閱讀大型 Codebase | Gemini 2.0 Flash |
| 快速小修改 | Claude Haiku / GPT-4o |
| 預算有限 | DeepSeek V3 |

> [!WARNING]
> **模型幻覺**：所有模型都可能編造不存在的 API。務必搭配 TypeScript 驗證。

---

## 6. 費用與資安風險 (Risks & Costs)

### 費用

| 項目 | 說明 |
| :--- | :--- |
| **API Token** | 大量使用 Agent 模式時消耗極快，關注 Dashboard 避免爆預算 |
| **訂閱費** | Cursor Pro / Vercel Pro 約 $20 USD/月 |

### 資安與隱私

| 風險 | 對策 |
| :--- | :--- |
| **敏感資料外洩** | 絕不將 API Keys 傳送給 AI，使用 `.env` 管理 |
| **代碼隱私** | 處理公司機密時詳閱隱私條款或開啟 Private Mode |

---

## 7. 常見錯誤與陷阱 (Common Pitfalls)

即使有 AI 輔助，以下錯誤仍會讓專案陷入困境：

### 開發流程錯誤

| ❌ 錯誤 | ✅ 正確做法 |
| :--- | :--- |
| 完全信任 AI 產出，不做 Code Review | 每次 AI 產出都要人工檢查邏輯與安全性 |
| 沒有用 Git Commit，AI 改壞了回不去 | 每完成一個功能就 Commit，保留 rollback 能力 |
| 一次給 AI 太大的任務 | 拆解成小模組，逐步完成 |
| 不看錯誤訊息，直接叫 AI 修 | 先理解錯誤原因，再請 AI 協助 |

### Prompt 錯誤

| ❌ 錯誤 | ✅ 正確做法 |
| :--- | :--- |
| 「幫我修好這個 bug」 | 「這個按鈕點擊後沒有反應，console 顯示 X 錯誤，請修正」 |
| 不提供上下文 | 使用 `@Files` 附上相關檔案，或貼上錯誤訊息 |
| 期待一次完美 | 迭代優化：「不錯，但請把按鈕改成藍色」 |

### 資安錯誤

| ❌ 錯誤 | ✅ 正確做法 |
| :--- | :--- |
| 把 API Key 寫在程式碼裡 | 使用 `.env` 並加入 `.gitignore` |
| 把含有密碼的程式碼傳給 AI | 移除敏感資訊後再請 AI 協助 |
| 不看 AI 引入的 npm 套件 | 檢查套件來源與下載量，避免惡意套件 |

> [!CAUTION]
> **技術債累積**：AI 寫得快不代表寫得好。定期重構，不要讓「能跑就好」變成習慣。

---

## 8. 教學資源 (Resources)

### 官方文件

| 資源 | 連結 |
| :--- | :--- |
| React | [react.dev](https://react.dev/) |
| Next.js | [nextjs.org/docs](https://nextjs.org/docs) |
| Cursor | [docs.cursor.com](https://docs.cursor.com/) |

### YouTube 頻道

| 頻道 | 內容 |
| :--- | :--- |
| Traversy Media | 基礎 Web 開發觀念 |
| Web Dev Simplified | 基礎 Web 開發觀念 |
| Mckay Wrigley | AI Coding workflow 與 Cursor 技巧 |

### 社群

| 平台 | 說明 |
| :--- | :--- |
| Twitter (X) #buildinpublic | AI 開發者社群 |
| Cursor Forum / Discord | 官方社群 |

---

## 9. 進階心法與工作流 (Mindset & Workflow)

Vibe Coding 的核心是**角色轉換**——從「寫程式的人」變成「指揮 AI 的編排者」。

### 心態轉變

| 傳統開發 | Vibe Coding |
| :--- | :--- |
| 80% 時間寫語法 | 80% 時間釐清邏輯與架構 |
| 專注「如何寫」 | 專注「做什麼」與「為什麼」 |
| 一個人寫 Code | 監督 AI Agent 執行任務 |

### 關鍵實踐

| 實踐 | 說明 |
| :--- | :--- |
| **模組化思考** | 將大問題拆解成小 function，AI 準確度極高 |
| **Agentic Thinking** | 把 AI 當代理人，授權執行、你負責監督 |
| **Git 防波堤** | 每次 AI 迭代就 Commit，出錯時可 rollback |
| **機器驗證機器** | 要求 AI 為程式碼撰寫 Unit Test |
| **本地模型備案** | 資安敏感專案用 Ollama 運行本地模型 |

---

## 總結

Vibe Coding 是一種 **"Human Directs, AI Generates"** 的新工作流。

你的核心價值從「寫出語法正確的 Code」轉變為「定義正確的問題」與「判斷更好的解決方案」。
