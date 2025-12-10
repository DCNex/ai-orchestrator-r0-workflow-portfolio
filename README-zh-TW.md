# AI Orchestrator 系列文：R0 方法論、工作流與 Kaggle 教育案例

這個 Repo 紀錄的是一套實作過的 **AI Orchestrator / AI Workflow Architect 方法論**，以及如何在短時間內，用它完成像 Kaggle 黑客松這種高壓情境下的 MVP。

整個系列圍繞三個關鍵字：

- **R0 / SSOT**：用 Single Source of Truth 與 Core Kernel + Modules 管理專案知識與架構。
- **AI 調度**：不是「一個大模型做全部」，而是多工具、多模型、多步驟的分層協作。
- **實戰案例**：以 Kaggle「Vibe Code with Gemini 3 Pro」教育題目，做出 GapLens – Learning Loss Scanner。

---

## 目錄（Series Overview）

> 所有文章都有中英雙語版本。  
> 這份 README 是繁體中文版（`README-zh-TW.md`），英文版為 `README.md`。

1. **系列文第 1 篇：R0 / AI Orchestrator 方法論**  
   - 說明 R0 是什麼、為何需要 SSOT（Single Source of Truth）、Core Kernel + Modules 的結構，以及 L1–L10 工作層級。  
   - 檔案：  
     - 繁體中文：[`01-r0-ai-orchestrator-methodology-zh-TW.md`](./01-r0-ai-orchestrator-methodology-zh-TW.md)  
     - English：[`01-r0-ai-orchestrator-methodology.md`](./01-r0-ai-orchestrator-methodology.md)

2. **系列文第 2 篇：AI 調度溝通地圖（L1–L10）**  
   - 把 L1–L10 用來描述「人／團隊使用 AI 的成熟度」，並整理成一份溝通與交付物的 Playbook。  
   - 檔案：  
     - 繁體中文：[`02-ai-orchestrator-communication-map-zh-TW.md`](./02-ai-orchestrator-communication-map-zh-TW.md)  
     - English：[`02-ai-orchestrator-communication-map.md`](./02-ai-orchestrator-communication-map.md)

3. **系列文第 3 篇：AI Orchestrator MVP → 上線工作流指南**  
   - 從題目選定 → MVP → Demo → 部署 → 上線溝通 → 迭代的標準流程，說明每一步 R0 與各種 AI 工具怎麼分工。  
   - 檔案：  
     - 繁體中文：[`03-ai-orchestrator-mvp-launch-workflow-zh-TW.md`](./03-ai-orchestrator-mvp-launch-workflow-zh-TW.md)  
     - English：[`03-ai-orchestrator-mvp-launch-workflow.md`](./03-ai-orchestrator-mvp-launch-workflow.md)

4. **系列文第 4 篇：Kaggle 教育案例 – GapLens 學習落差掃描器**  
   - 在「Vibe Code with Gemini 3 Pro」黑客松中，以 Education 類別做出的 MVP。  
   - 展示如何用 Gemini 3 Pro + Google AI Studio 實作 GapLens，並整理 AI Studio 中常見問題與 Prompt 設計原則。  
   - 檔案：  
     - 繁體中文：[`04-kaggle-gaplens-education-case-zh-TW.md`](./04-kaggle-gaplens-education-case-zh-TW.md)  
     - English：[`04-kaggle-gaplens-education-case.md`](./04-kaggle-gaplens-education-case.md)

---

## 建議閱讀路徑

如果是第一次接觸這套方法論，可以參考以下順序：

1. **先看第 1 篇**：了解 R0 / SSOT 與 Core + Modules 的概念。  
2. **再看第 2 篇**：理解 L1–L10 成熟度，方便在團隊裡溝通 AI 題目與預期成果。  
3. **接著看第 3 篇**：把整條 「MVP → 上線」 工作流走一遍。  
4. **最後看第 4 篇**：把前面三篇的概念，實際套在 Kaggle 教育案例 GapLens 上。

如果只對實戰案例有興趣，也可以：

- 先看第 4 篇了解 GapLens，在需要時再回頭查第 1–3 篇，當作「理論與工作流的參考手冊」。

---

## 使用場景與關鍵字（GitHub SEO）

本系列特別適合：

- 想成為 **AI Orchestrator / AI Workflow Architect / AI PM** 的人。  
- 需要在有限時間內，交出 **可以 Demo、可以上 Kaggle / Hackathon / 面試** 的 AI MVP。  
- 想把多個 AI 工具（ChatGPT / Gemini / Claude / Perplexity / Suno / TTS / Canva …）變成一條真正可重複的工作流。

關鍵字（方便搜尋）：

- AI Orchestrator, AI Workflow Architect, R0 Protocol  
- Single Source of Truth（SSOT）, Core Kernel, Modules  
- L1–L10 AI Maturity, AI Communication Map  
- MVP Launch Workflow, AI Product Management  
- Google Gemini 3 Pro, Google AI Studio, Kaggle Hackathon  
- Education AI, Learning Loss, GapLens

---

## 授權與聯絡方式

- 本 Repo 目前以個人作品集與教學用為主，如需商業合作或共同開發，歡迎來信。  
- 聯絡方式：**dksitn@gmail.com**
