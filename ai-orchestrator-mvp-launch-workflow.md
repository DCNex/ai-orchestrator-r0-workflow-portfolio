# AI Orchestrator MVP→上線工作流指南（系列文 · 第 3 篇）

English | [繁體中文 / Traditional Chinese](./ai-orchestrator-mvp-launch-workflow-zh-TW.md)

[English Version](./ai-orchestrator-mvp-launch-workflow.md)

---

## 0. 系列地圖：本篇在整體中的位置

本 Repo 目前有三條主線文件：

1. **系列文第 1 篇：Arin 的 R0 / AI Orchestrator 方法論**  
   - 說明 R0 是什麼、為何需要 SSOT（Single Source of Truth）、Core Kernel + Modules 的結構，以及 L1–L10 工作層級。  
   - 檔案：`r0-ai-orchestrator-methodology-zh-TW.md`

2. **系列文第 2 篇：AI 調度溝通地圖（L1–L10）**  
   - 說明如何用 L1–L10 來描述「人／團隊使用 AI 的成熟度」，以及 AI 應用 PM 面對不同 Level 的溝通方式與交付物。  
   - 檔案：`ai-orchestrator-communication-map-zh-TW.md`

3. **系列文第 3 篇：本文件（MVP→上線工作流指南）**  
   - 聚焦實作流程：  
     - 題目選定 → MVP 設計 → Demo 產出 → 開發與部署 → 上線溝通 → 觀測與迭代  
   - 說明：  
     - R0、Core 與 Modules 在流程中的角色  
     - 各階段使用哪些 AI 工具，以及為什麼這樣分工

可以把三篇合起來視為：

- 第 1 篇：**系統架構與規則說明書**  
- 第 2 篇：**與人溝通、推動專案的地圖**  
- 第 3 篇（本篇）：**從 0 到上線的實作 SOP**

---

## 1. 使用場景與角色

### 1.1 適用對象

這條工作流主要服務以下情境：

- 需要用多個 AI 工具完成部門專案或內部流程改善的人  
- 想把 side project 做成「可展示、可實際使用」作品的人  
- 準備走向 AI Orchestrator / AI Workflow Architect / AI PM 角色的人

### 1.2 典型題目範例

- 訂閱管理工具、數位資產庫、個人財務儀表板  
- 城市情報地圖（例如嫌惡設施、寵物友善地點等）  
- 部門績效與獎酬制度的分析與可視化工具  
- Remote Life OS 相關模組的 Demo

---

## 2. 工具分工一覽（分層而不是亂用）

### 2.1 規劃與結構層

- **ChatGPT**  
  - 用途：需求拆解、流程設計、資料結構（JSON / schema）草稿、PRD／技術說明撰寫、提示詞設計。  

- **Claude**  
  - 用途：閱讀／重構程式碼、找 bug、協助改寫較大段實作。  

- **Perplexity**  
  - 用途：市場／競品／技術與行為研究，支撐設計與商業假設。  

- **NotebookLM**  
  - 用途：整理某個題目的文件、對話與筆記，變成專案知識庫。

### 2.2 UI / 視覺層

- **Google AI Studio**：快速生成 App／Web 假畫面與操作流程。  
- **Gemini**：封面、圖示、UI 風格與版型建議。

### 2.3 影片與聲音層

- **Sora / Kling / 海螺**：生成操作 Demo 或情境影片。  
- **Suno**：生成背景音樂。  
- **TTSMaker**：將腳本轉成配音（可多語版本）。  
- **Canva**：剪輯影片、排版字幕與資訊卡，輸出不同平台尺寸。

### 2.4 在地化與文化層

- **豆包（Doubao）**：將既有文案調整為較貼近大陸網路語境的版本，作為在地化參考。

---

## 3. R0 視角下的七階段工作流

以下是我實際用在部門專案與 side project 的標準流程。  
每一階段會說明：

- R0 要做什麼  
- 會用到哪些 AI 工具  
- 應該產出什麼成果

---

### 第 0 階段：題目與方向確認

**目標**  
決定這一輪要做的題目、主要對象與時間箱（例如 4–8 週）。

**R0 的任務**

- 在 SSOT 中建立「實驗卡」，包含：  
  - 題目名稱  
  - 目標族群／Persona  
  - 期間（例如 4–8 週）  
  - 粗略成功條件（例如：有 1 支 Demo 影片＋1 個可分享的網址）

**AI 的使用方式**

- ChatGPT：協助拆題，列出不同 Persona 的痛點與使用情境。  
- Perplexity：搜尋現有解法與競品。  
- 豆包：將核心訴求轉成大陸語境文案，觀察敘事差異。

---

### 第 1 階段：Problem / Market Fit 初步檢查

**目標**  
判斷該題目是否值得投入做 MVP。

**R0 的任務**

- 把本輪的「假設」與「成功／失敗條件」寫入 SSOT：  
  - 例如：若 N 週內達到多少使用、多少有效回饋，即視為通過。

**AI 的使用方式**

- Perplexity：整理競品、常見使用情境與定價模式。  
- ChatGPT：將資料整理成一頁「市場快照」與「我的切入角度」。

產出建議：一份 Markdown／Notion 頁面，可在系列文或案例中引用。

---

### 第 2 階段：MVP 與體驗流程設計（含文化考量）

**目標**  
用文字把 v0.1 說清楚，只專注在一個最重要的工作情境。

**R0 的任務**

- 指定或新增模組（例如 `M02_Subscription`、`M03_CityIntel`）。  
- 定義：  
  - 這一輪需要哪些核心資料欄位  
  - 模組與 Core 的 API 介面與讀寫權限

**AI 的使用方式**

- ChatGPT：  
  - 產出 MVP 功能清單與 step-by-step 使用流程。  
- ChatGPT（不同 Persona 設定）：  
  - 模擬不同文化、不同職位使用者走流程，標註可能卡關點。  
- 豆包：  
  - 對大陸市場版本調整關鍵文案與流程描述。

產出建議：一份「MVP 使用劇本＋流程圖」，可直接給設計與開發閱讀。

---

### 第 3 階段：Prototype（假 UI）與 Demo 故事線

**目標**  
在寫程式前，先讓所有人看得懂產品長什麼樣、怎麼用。

**R0 的任務**

- 確認使用流程與資料流沒有邏輯衝突。  
- 保持視覺層中立，不干預顏色與細部樣式。

**AI 的使用方式**

- Google AI Studio：生成 App／Web 假畫面與互動流程。  
- Gemini：優化畫面、配色與基本排版建議。  
- ChatGPT：撰寫 Demo 旁白稿與故事線（可規劃中英雙語）。

---

### 第 4 階段：Demo 影片製作

**目標**  
產出一支可以放在簡報、GitHub、社群平台的 Demo 影片。

**R0 的任務**

- 確認 Demo 沒有誇大，內容在合理的技術與商業範圍內。

**AI 的使用方式**

- Sora / Kling / 海螺：生成操作畫面或情境影片。  
- TTSMaker：根據腳本產生配音。  
- Suno：製作背景音樂。  
- Canva：剪輯影片、加上字幕與註解，輸出多種比例（X／Threads／IG／Shorts）。

---

### 第 5 階段：開發與部署

**目標**  
實作出可實際使用的 MVP，並部署到可分享的網址。

**R0 的任務**

- 更新 SSOT：  
  - schema（資料表結構）  
  - API 介面  
  - `modules_map` 與 `integration_registry`  
- 審查 AI 產出的程式碼是否符合：  
  - 模組邊界  
  - 安全與維護性要求

**AI 的使用方式**

- ChatGPT：  
  - 協助設計資料表與 API、撰寫初版程式碼。  
- Claude：  
  - 協助閱讀／重構／除錯。  
- NotebookLM：  
  - 收納關鍵程式片段與設計決策，成為技術知識庫。

---

### 第 6 階段：上線說明與對外溝通

**目標**  
讓目標使用者、主管或合作對象理解這個工具在做什麼，以及為何重要。

**R0 的任務**

- 設定本輪對外溝通的主要目的：  
  - 收集回饋  
  - 作為作品集  
  - 為下一階段鋪路等

**AI 的使用方式**

- ChatGPT：  
  - 撰寫 README／Landing Page、短版貼文與一句話標語。  
- Perplexity：  
  - 提供相關領域的關鍵字與常見搜尋語。  
- 豆包：  
  - 完成大陸市場版本文案。  
- Gemini + Canva：  
  - 產出宣傳圖、流程圖與簡報頁面。

---

### 第 7 階段：數據與回饋 → 下一步決策

**目標**  
根據實際使用與回饋，決定要加碼、調整方向或暫停。

**R0 的任務**

- 將實際結果與 SSOT 中的「假設與成功條件」比對，  
  做出：繼續、轉向或結束的決策。

**AI 的使用方式**

- ChatGPT：  
  - 協助整理使用數據與回饋，區分：  
    - 功能／體驗問題  
    - 文化／溝通問題  
- NotebookLM：  
  - 將本輪專案整理成「可重複引用的案例」，供未來專案或對外說明使用。

---

## 4. 如何與其他系列文件搭配閱讀？

- 想理解 **為何要有 R0／SSOT／Core+Modules 與 L1–L10 工作層級** →  
  請參考：`r0-ai-orchestrator-methodology-zh-TW.md`

- 想知道 **面對不同 AI 使用成熟度（L1–L10）的對象，怎麼溝通與交付** →  
  請參考：`ai-orchestrator-communication-map-zh-TW.md`

- 想知道 **實際一輪從題目到 MVP、上線與迭代要怎麼跑** →  
  就以本文件作為操作清單。

三篇文件組合在一起，構成：

1. 架構與協議層（R0 / SSOT / L1–L10 工作層級）  
2. 溝通與利害關係人管理層（L1–L10 AI 使用成熟度）  
3. 執行與實作層（0→MVP→上線→迭代）

---

# AI Orchestrator MVP→Launch Workflow Guide (Series · Part 3)

[English](./ai-orchestrator-mvp-launch-workflow.md) | [繁體中文 / Traditional Chinese](./ai-orchestrator-mvp-launch-workflow-zh-TW.md)

---

## 0. Series map: where this guide fits

This repository currently has three core documents:

1. **Series Part 1: Arin’s R0 / AI Orchestrator Methodology**  
   - Defines R0, SSOT (Single Source of Truth), the Core Kernel + Modules structure, and the L1–L10 work layers.  
   - File: `r0-ai-orchestrator-methodology.md`

2. **Series Part 2: AI Orchestrator Communication Map (L1–L10)**  
   - Uses L1–L10 to describe how mature people/teams are in using AI, and gives a playbook for AI Product/Project roles.  
   - File: `ai-orchestrator-communication-map.md`

3. **Series Part 3: this guide (MVP→Launch workflow)**  
   - Focuses on execution:  
     - Topic selection → MVP design → demo production → development & deployment → launch communication → observation & iteration  
   - Explains:  
     - How R0, Core and Modules share responsibilities  
     - Which AI tools are used at each stage, and why

You can think of the three as:

- Part 1 – **Architecture & rules**  
- Part 2 – **People & communication map**  
- Part 3 – **End-to-end execution SOP**

---

## 1. Roles and use cases

### 1.1 Who this workflow is for

This workflow is intended for:

- People using multiple AI tools to deliver internal projects or process improvements  
- People turning side projects into usable, presentable products  
- Anyone moving toward an AI Orchestrator / AI Workflow Architect / AI PM role

### 1.2 Typical topics

- Subscription manager, digital asset library, personal finance dashboards  
- City intelligence maps (e.g. nuisance facilities, pet-friendly places)  
- Internal performance / incentive dashboards and workflow tools  
- Remote Life OS modules and public demos

---

## 2. Tool roles (structured, not random)

### 2.1 Planning & structure

- **ChatGPT**  
  - Uses: requirements breakdown, workflow design, data structure drafts (JSON / schema), PRD and technical documentation, prompt design.  

- **Claude**  
  - Uses: reading/refactoring code, debugging, larger implementation refactors.  

- **Perplexity**  
  - Uses: market/competitor/technical research to support design and business assumptions.  

- **NotebookLM**  
  - Uses: project knowledge base for a specific topic (docs, notes, conversations).

### 2.2 UI / visual

- **Google AI Studio** – fast app/web mock screens and flows.  
- **Gemini** – covers, icons, UI style and layout suggestions.

### 2.3 Video & audio

- **Sora / Kling / Hailuo** – product demo or scenario video generation.  
- **Suno** – background music.  
- **TTSMaker** – voice-over from scripts (multi-language).  
- **Canva** – editing, subtitles, info cards, multi-format export.

### 2.4 Localization & culture

- **Doubao** – Mainland China style copy and phrasing for localization.

---

## 3. Seven-stage workflow from the R0 perspective

Below is the workflow I actually use for internal projects and side projects.  
Each stage describes:

- What **R0** is responsible for  
- Which **AI tools** are typically involved  
- What **artifacts** should be produced

---

### Stage 0 – Topic & direction

**Goal**  
Decide what to build in this cycle, for whom, and within what time box (e.g. 4–8 weeks).

**R0 responsibilities**

- Create an experiment card in SSOT containing:  
  - Topic name  
  - Target audience / persona  
  - Time window (e.g. 4–8 weeks)  
  - Rough success criteria (e.g. 1 demo video + 1 shareable URL)

**AI usage**

- ChatGPT: topic breakdown, personas and pain points.  
- Perplexity: existing solutions and competitors.  
- Doubao: alternate framing for Mainland audiences to see differences in narrative.

---

### Stage 1 – Initial Problem / Market Fit check

**Goal**  
Decide whether the topic deserves an MVP.

**R0 responsibilities**

- Record hypotheses and success/failure conditions in SSOT, such as:  
  - If we reach certain usage or feedback within N weeks, the experiment is considered successful.

**AI usage**

- Perplexity: list competitors, typical use cases, pricing patterns.  
- ChatGPT: assemble a one-page market snapshot and angle-of-entry summary.

Result: a short Markdown/Notion page you can later reference as part of your case.

---

### Stage 2 – MVP & experience design (with cultural angle)

**Goal**  
Describe v0.1 clearly in text, focusing on one critical work scenario.

**R0 responsibilities**

- Assign or create a module (e.g. `M02_Subscription`, `M03_CityIntel`).  
- Define:  
  - Required core fields for this cycle  
  - Module–Core interfaces and permissions

**AI usage**

- ChatGPT:  
  - MVP feature list and step-by-step flow.  
- ChatGPT with personas:  
  - Simulated flows for different cultures/roles; highlight friction points.  
- Doubao:  
  - Adjust key copy and flow descriptions for Mainland China.

Suggested output: an “MVP script + flow diagram” that design and dev can read directly.

---

### Stage 3 – Prototype (mock UI) & demo storyline

**Goal**  
Provide screens and story before any real code is written.

**R0 responsibilities**

- Check alignment between user flows and data flows; stay neutral on colors and visual details.

**AI usage**

- Google AI Studio: mock app/web screens and basic interactions.  
- Gemini: refine visuals, color hints and basic layout.  
- ChatGPT: draft demo narration and storyline (often bilingual).

---

### Stage 4 – Demo video production

**Goal**  
Produce a demo video for slides, GitHub and social channels.

**R0 responsibilities**

- Ensure the demo does not oversell beyond realistic technical and business scope.

**AI usage**

- Sora / Kling / Hailuo: generate operation or scenario footage.  
- TTSMaker: turn scripts into voice-over.  
- Suno: create background music.  
- Canva: edit, add subtitles and annotations, export to multiple aspect ratios (X / Threads / IG / Shorts).

---

### Stage 5 – Development & deployment

**Goal**  
Ship a working MVP to a public, shareable URL.

**R0 responsibilities**

- Update SSOT:  
  - Schema (tables)  
  - APIs  
  - `modules_map` and `integration_registry`  
- Review AI-generated code for:  
  - Respecting module boundaries  
  - Safety and maintainability

**AI usage**

- ChatGPT:  
  - Help design tables/APIs and write initial code.  
- Claude:  
  - Read, refactor, debug.  
- NotebookLM:  
  - Store key snippets and design decisions as a technical knowledge base.

---

### Stage 6 – Launch communication

**Goal**  
Make the right people understand what the tool does and why it matters.

**R0 responsibilities**

- Decide the primary communication goal for this cycle:  
  - Feedback, portfolio proof, preparing for a next phase, etc.

**AI usage**

- ChatGPT:  
  - Write README / landing copy, short posts, and a one-line tagline.  
- Perplexity:  
  - Suggest domain-relevant keywords and search terms.  
- Doubao:  
  - Finalize Mainland-facing copy.  
- Gemini + Canva:  
  - Produce visuals: promo images, diagrams, slide pages.

---

### Stage 7 – Data, feedback & next decisions

**Goal**  
Use real usage and feedback to decide whether to double down, adjust or pause.

**R0 responsibilities**

- Compare results with hypotheses and success criteria in SSOT,  
  then choose: continue, pivot, or stop.

**AI usage**

- ChatGPT:  
  - Summarize metrics and feedback into:  
    - Functional / UX issues  
    - Cultural / communication issues  
- NotebookLM:  
  - Turn this cycle into a reusable case for future projects and external storytelling.

---

## 4. How to combine this with the other series documents

- For **why** R0, SSOT, Core+Modules and L1–L10 levels exist →  
  read `r0-ai-orchestrator-methodology.md`.

- For **how to talk to people** at different AI maturity levels and what to deliver to each →  
  read `ai-orchestrator-communication-map.md`.

- For **how to actually run one full cycle from idea to MVP to launch and iteration** →  
  use this guide as an operational checklist.

Together, the three documents form:

1. Architecture & protocol (R0 / SSOT / L1–L10 work layers)  
2. Communication & stakeholder management (L1–L10 AI maturity)  
3. Execution workflow (0 → MVP → launch → iteration)
