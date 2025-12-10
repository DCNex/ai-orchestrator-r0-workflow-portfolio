# 系列文第 4 篇：Kaggle 教育案例 – GapLens 學習落差掃描器  
用 Gemini 3 Pro + AI Studio 做出「看得懂、交得出去」的教育 MVP

English | [繁體中文 / Traditional Chinese](./kaggle-gaplens-education-case-zh-TW.md)

---

## 0. 系列地圖：本篇在整體中的位置

這篇是「AI Orchestrator / R0 系列」的第 4 篇。

前面三篇是：

1. **第 1 篇：Arin 的 R0 / AI Orchestrator 方法論**  
   - 講 SSOT（Single Source of Truth）、Core Kernel + Modules、L1–L10 工作層級。  

2. **第 2 篇：AI 調度溝通地圖（L1–L10）**  
   - 把 L1–L10 拿來描述「人／團隊用 AI 的成熟度」，附一份溝通 Playbook。  

3. **第 3 篇：AI Orchestrator MVP→上線工作流指南**  
   - 說明從題目選定 → MVP → Demo → 上線 → 迭代的標準流程。  

> 可以把前三篇視為：  
> - 架構與規則、  
> - 與人溝通的地圖、  
> - 從 0 到上線的 SOP。  
>  
> 本篇則是：**把 SOP 實際套在一個 Kaggle 黑客松教育題目上的完整案例**。

---

## 1. 案例概觀：GapLens 是什麼？

### 1.1 題目背景

- 比賽：**Vibe Code with Gemini 3 Pro**（Kaggle 快閃黑客松）  
- 類別：**Education / Learning**  
- 要求：
  - 必須在 **Google AI Studio – Build** 介面完成 App；
  - 充分運用 **Gemini 3 Pro 的進階推理＋原生多模態**；
  - 提交 Kaggle Writeup、公開 App 連結與 2 分鐘 Demo 影片。

### 1.2 GapLens 的核心想法

**GapLens – Learning Loss Scanner** 的定位：

> 給老師／補救教學者使用的「學習落差掃描器」，  
> 讓他們只要上傳考卷或筆記，就能看出學生「概念錯在哪裡」，  
> 並一鍵生成差異化的練習與回家作業。

重點不是做一個「全功能題庫系統」，而是：

- 幫老師把「**看答案、猜學習落差**」這一步自動化；
- 用 AI 幫忙分類：
  - 概念錯誤（Conceptual）
  - 步驟／程序錯誤（Procedural）
  - 粗心錯誤（Careless）
- 直接給出：
  - 個別學生的錯誤摘要與個人作業包；
  - 班級層級的共通落差與小組建議。

---

## 2. 從 R0 角度看：AI Studio 這類工具常見的 5 類問題

在用 Gemini + AI Studio 做這個 MVP 的過程，我刻意用 R0 視角整理了一份「**原則級問題清單**」，目的是讓沒看過這個案例的人，也能直接拿去套在自己的教育 MVP 上。

每一條都包含：

- 問題背景（原則化描述）  
- 為何在教育場景會變成風險  
- 對應的 Prompt 設計原則

### 2.1 語言錯置：中英文混雜 / 風格不一致

**問題背景**

- 輸入考卷是中文，但 AI 會把部分內容原文保留，造成：  
  - 題目變成「Find the value of A 減去 B 的倒數」這種混語句。  
- 對國際評審或學生來說，會瞬間掉出情境。

**風險（教育場景）**

- 學生不一定懂兩種語言，容易誤解題目。  
- Kaggle / Hackathon 評審在短時間內判斷產品專業度時，語言一致性是基本門檻。

**Prompt 設計原則**

- 一開始就定義好 UI 語言政策：

  > `All user-facing content in this app must be in English only.`  
  > `When the input exam or notes are in Chinese, read them, interpret them, and then restate all questions, errors and explanations in English.`  
  > `Do not mix languages in a single sentence and do not output any non-English characters in the UI.`

---

### 2.2 內部內容外洩：開發者評論 / JSON / 系統說明跑到畫面上

**問題背景**

- AI Studio 的同一個 Prompt 裡，同時塞了：
  - JSON schema、欄位說明；
  - 開發註解（例如「這裡請不要改欄位名稱」）；
  - 示範資料與使用者的考卷內容。  
- 如果沒有約束，Gemini 會把這些「對開發者說的話」連同 JSON 一起輸出到 UI。

**風險**

- 考卷區塊突然出現「This is excellent and follows JSON schema rules…」這種內容。  
- 對學生、老師、評審來說，直接判定為「半成品」。

**Prompt 設計原則**

- 把「不能出現在 UI」的東西寫死：

  > `You are generating content for teachers and students. Never mention prompts, JSON schemas, developer notes, or system messages in any user-facing field.`  
  > `Do not output JSON, code blocks, or backticks in questions, explanations, or summaries.`

- 對每個欄位定義用途（尤其在 AI Studio）：

  > `The 'question' field contains only the problem statement.`  
  > `The 'explanation' field contains only the explanation for a student, with no internal comments.`  

---

### 2.3 內容過長與欄位混用：一個欄位塞滿評論＋解題＋其它題目

**問題背景**

- 模型傾向「一次把所有 helpful 的東西講完」，結果：
  - 一題的敘述同時包含三題的解析；
  - 錯誤分析裡夾雜對老師的教學建議、對開發者的稱讚。

**風險**

- UI 排版難以控制，手機端完全看不完；  
- 老師很難一眼看出「學生到底哪裡不會」。

**Prompt 設計原則**

- 明確限制每個欄位的長度與結構：

  > `Limit each explanation to at most 3 short sentences: (1) what went wrong, (2) the correct idea, (3) a short tip.`  
  > `Do not restate the full problem text inside the explanation.`  

---

### 2.4 功能範圍模糊：MVP 只有一個學生，畫面卻長得像完整 LMS

**問題背景**

- 構想中包含：
  - 班級視圖、分組建議；
  - 班級柱狀圖、學習落差排行等等。  
- 但在 AI Studio MVP 階段，實際可行的是：
  - 一次上傳 1–N 個學生的考卷；
  - 自動聚合簡單的「Class View」而已。

**風險**

- 如果畫面出現大量「Coming soon」「Locked」「Fake Student A/B/C」，會被誤判為 Bug。  
- Hackathon 的時間非常短，評審看不到真實價值，只有沒有完成的野心。

**Prompt 設計原則**

- 讓模型知道這一版只有 **Exam Mode + Class View MVP**：

  > `For this MVP, focus on Exam Mode for individual students and a lightweight Class View based on uploaded exams. Do not render any extra tabs or features that are not described below.`  

- 未來想做的東西只放在側邊說明文字，不畫假按鈕。

---

### 2.5 UI 可用性：窄欄＋內部捲軸，看不到完整字

**問題背景**

- 初版設計會在右側塞一整欄錯題列表，並在每張卡片內再加一層 scrollbar。  
- 結果使用者要「整頁滾動 + 卡片內再滾動」才能看到完整句子。

**風險**

- Demo 錄影時很難一次展示完關鍵資訊；  
- 手機 / 小螢幕體驗會直接崩潰。

**Prompt 設計原則**

- 在提示詞裡寫清楚版型原則，而不是讓 AI 隨意排版：

  > `Design the UI with a single vertical scroll. Cards should expand to fit their content; do not use inner scrollbars inside cards.`  
  > `For long texts (error lists, homework items), use collapsible sections with a short one-line summary when collapsed.`  

---

## 3. GapLens 的 MVP 設計：資料結構與使用流程

### 3.1 使用角色與模式

**角色**

- Teacher / Educator（主要使用者）  
- Student（只看到老師產出的結果，這版不需要登入）

**兩個模式**

1. **Exam Mode – Diagnose and remediate from graded papers**  
   - 老師上傳數張拍好的考卷照片（同一科目、同一班級）。  
   - Gemini 解析題目與學生答案，判斷每一題的錯誤類型。  
   - 輸出：
     - 個別學生錯誤摘要；
     - 差異化回家作業包；
     - 班級層級的共通落差與小組建議。

2. **Note Mode – Extend learning from notes**  
   - 老師或學生上傳上課筆記／板書照片。  
   - Gemini 幫忙整理：
     - 概念地圖；
     - 題組練習；
     - 延伸閱讀或題型。

> 黑客松版本主要 Demo **Exam Mode**，Note Mode 以 UI 骨架與故事線方式簡單展示。

---

### 3.2 資料結構（概念級）  

以下是簡化後的概念結構，用來跟 AI 解釋「你可以創造哪些東西、不要亂加欄位」。

```jsonc
{
  "student": {
    "id": "S01",
    "alias": "Student A",
    "grade": "9",
    "subject": "Algebra"
  },
  "exam": {
    "id": "E2025-12-Midterm",
    "questions": [
      {
        "qId": "Q1",
        "label": "Q1",
        "topic": "Linear equations",
        "skills": ["equation setup", "one-variable solving"]
      }
    ]
  },
  "responses": [
    {
      "studentId": "S01",
      "questionId": "Q1",
      "rawAnswer": "…",
      "errorType": "Conceptual | Procedural | Careless",
      "errorTags": ["equation setup", "sign error"],
      "explanation": "Short student-facing explanation."
    }
  ],
  "studentSummary": {
    "studentId": "S01",
    "score": 18.5,
    "identifiedGaps": [
      "Consistency in subject–verb agreement and noun plurality.",
      "Accurate use of different word forms."
    ],
    "homeworkPack": [
      {
        "qId": "HW1",
        "prompt": "Rewrite these sentences with correct subject–verb agreement.",
        "mode": "text"
      }
    ]
  },
  "classSummary": {
    "topGaps": [
      { "topic": "Quadratic factoring", "severity": 0.8 },
      { "topic": "Sign errors", "severity": 0.6 }
    ],
    "groupSuggestions": [
      {
        "groupLabel": "Factoring Recovery",
        "students": ["S01", "S03", "S05"],
        "sessionPlan": "10min GCF recap, 20min worksheet."
      }
    ]
  }
}
