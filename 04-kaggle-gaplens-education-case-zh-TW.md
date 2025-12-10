# 系列文第 4 篇：Kaggle 教育案例 – GapLens 學習落差掃描器  
用 Gemini 3 Pro + AI Studio 做出可 Demo 的教育 MVP

English: [04-kaggle-gaplens-education-case.md](./04-kaggle-gaplens-education-case.md)  
繁體中文：本篇

---

## 0. 系列地圖：本篇在整體中的位置

本篇是「AI Orchestrator / R0 系列」的第 4 篇。

前三篇依序是：

1. **第 1 篇：R0 / AI Orchestrator 方法論**  
   說明 SSOT、Core Kernel + Modules、L1–L10 工作層級等底層概念。

2. **第 2 篇：AI 調度溝通地圖（L1–L10）**  
   用 L1–L10 描述「人／團隊使用 AI 的成熟度」，整理出溝通與交付物的 Playbook。

3. **第 3 篇：MVP → 上線工作流指南**  
   從題目選定 → MVP → Demo → 部署 → 上線溝通 → 迭代的完整流程。

本篇的角色是：

- 把前面三篇的方法論，實際套在一個 Kaggle 黑客松教育題目上。  
- 示範如何用 Gemini 3 Pro + Google AI Studio 做出可 Demo 的教育 MVP：**GapLens – Learning Loss Scanner**。  
- 整理 AI Studio / Gemini 在教育場景常見的 5 類問題，對應的 Prompt 設計原則。

---

## 1. 案例概觀：GapLens 是什麼？

### 1.1 比賽背景

- 平台：Kaggle  
- 活動名稱：**Vibe Code with Gemini 3 Pro**  
- 類別：  
  - Science  
  - Education  
  - Accessibility  
  - Health  
  - Business  
  - Tech Breakthroughs  
- 開發要求：  
  - 必須在 **Google AI Studio – Build** 環境中完成 App。  
  - 必須使用 **Gemini 3 Pro**，並善用其進階推理與原生多模態能力。  
  - 需提交三項成果：  
    - Kaggle Writeup（專案說明）  
    - 可公開存取的 AI Studio App 連結  
    - 不超過 2 分鐘的 Demo 影片

### 1.2 GapLens 的核心定位

**GapLens – Learning Loss Scanner** 的定位：

> 這是一個給老師與補救教學者使用的「學習落差掃描器」。  
> 老師只要上傳紙本考卷或學習筆記，系統就會標出學生「概念錯在哪裡」，  
> 並一鍵產生差異化作業（personalized homework pack）。

設計重點在於：

- 不做完整 LMS、不做複雜題庫系統。  
- 聚焦在老師日常最耗腦的一步：「看答案 → 判斷學生到底不懂哪裡」。  
- 用 AI 協助將錯誤拆成三類：  
  - Conceptual error（概念錯誤）  
  - Procedural error（步驟／程序錯誤）  
  - Careless error（粗心錯誤）  
- 讓老師能快速看到：  
  - 個別學生的錯誤摘要與個人作業建議。  
  - 班級層級的「共通落差」與「可分組練習」方向（這部分可以作為未來延伸）。

---

## 2. R0 視角：AI Studio + Gemini 在教育場景的 5 類常見問題

在用 Gemini 3 Pro + AI Studio 迭代 GapLens 的過程中，實際踩到多種「系統級問題」。  
如果只看單一錯誤，很容易以為是模型不穩定；  
但從 R0 / Orchestrator 視角看，其實可以整理成 5 類「原則級問題」。

下面每一類都包含：

- 問題背景  
- 風險說明  
- 對應的 Prompt 設計原則

### 2.1 語言錯置：中英文混雜、風格不一致

**問題背景**

- 輸入的考卷可能是中文，AI 卻會在 UI 中同時輸出中文與英文。  
- 有時錯誤分析中文、題目英文、摘要混雜兩者。

**風險**

- 學生不一定同時熟悉兩種語言，容易誤解題目與解析。  
- 對國際評審或外部合作對象來說，會產生「尚未完成」或「不專業」的印象。

**Prompt 設計原則**

在 System Prompt 中寫死語言政策，例如：

```text
All user-facing content in this app must be in English only.
When the input exam or notes are in Chinese, read and interpret them,
then restate all questions, errors and explanations in English.
Do not mix languages in a single sentence and do not output any non-English characters in the UI.
