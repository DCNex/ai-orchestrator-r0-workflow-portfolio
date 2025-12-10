# 系列文第 4 篇：Kaggle 教育案例 – GapLens 學習落差掃描器  
用 Gemini 3 Pro + AI Studio 做出可 Demo 的教育 MVP

English: [04-kaggle-gaplens-education-case.md](./04-kaggle-gaplens-education-case.md)  
繁體中文：本篇

---

## 0. 系列地圖：本篇在整體中的位置

本篇是「AI Orchestrator / R0 系列」的第 4 篇。

前三篇分別是：

1. **第 1 篇：R0 / AI Orchestrator 方法論**  
   - SSOT、Core Kernel + Modules、L1–L10 工作層級。  
2. **第 2 篇：AI 調度溝通地圖（L1–L10）**  
   - 用 L1–L10 描述 AI 使用成熟度，提供溝通與交付物的 Playbook。  
3. **第 3 篇：MVP → 上線工作流指南**  
   - 從題目選定 → MVP → Demo → 上線 → 迭代的標準流程。

本篇要做的事是：

> 把前面三篇的概念，實際套在一個 Kaggle 黑客松教育題目上，  
> 並整理出 AI Studio + Gemini 3 Pro 在教育場景常見的問題與 Prompt 設計原則。

---

## 1. 案例概觀：GapLens 是什麼？

### 1.1 比賽背景

- 平台：Kaggle  
- 活動名稱：**Vibe Code with Gemini 3 Pro**  
- 類別：包含 Science / Education / Accessibility / Health / Business / Tech Breakthroughs 等  
- 開發限制：  
  - 必須在 **Google AI Studio – Build** 介面完成 App；  
  - 必須使用 **Gemini 3 Pro**，並善用進階推理與原生多模態能力；  
  - 需提交 Kaggle Writeup、公開 App 連結與 2 分鐘 Demo 影片。

### 1.2 GapLens 的核心定位

**GapLens – Learning Loss Scanner** 的定位是：

> 給老師與補救教學者使用的「學習落差掃描器」。  
> 只要上傳考卷或筆記，系統就會標出學生「概念錯在哪裡」，  
> 並一鍵產生差異化作業（針對不同學生的個人化練習）。

重點不是：

- 做一個完整的 LMS 或題庫系統；  

而是：

- 自動化老師「看答案 → 猜學生不懂哪裡」這一步；  
- 幫教師拆解錯誤成幾類：  
  - 概念錯誤（Conceptual）  
  - 步驟／程序錯誤（Procedural）  
  - 粗心錯誤（Careless）  
- 讓老師能一眼看到：  
  - 個別學生的錯誤摘要與建議作業包；  
  - 整個班級的共通落差與小組建議。

---

## 2. R0 視角：AI Studio + Gemini 在教育場景常見的 5 類問題

在用 Gemini 3 Pro + AI Studio 迭代 GapLens 的過程中，我刻意用 R0 視角整理出 5 類「原則級問題」，並為每一類配對 Prompt 設計原則。

這份清單可以被拿去直接套在其他教育 MVP 上。

---

### 2.1 語言錯置：中英文混雜、風格不一致

**問題背景**

- 輸入考卷是中文，AI 卻會輸出中英混雜的題目或解釋。  
- 對國際評審或學生而言，會造成理解負擔與專業感下降。

**風險**

- 學生不一定同時掌握兩種語言，容易誤解題目。  
- Kaggle / Hackathon 評審在短時間內判斷專案品質時，語言一致性是基本門檻。

**Prompt 設計原則**

- 在系統提示裡明確定義 UI 語言政策：

```text
All user-facing content in this app must be in English only.
When the input exam or notes are in Chinese, read and interpret them,
then restate all questions, errors and explanations in English.
Do not mix languages in a single sentence and do not output any non-English characters in the UI.
