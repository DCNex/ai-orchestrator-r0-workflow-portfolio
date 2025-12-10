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
```


重點是：

明確指定「使用者看到的所有內容」的語言。

強調「輸入可以是多語，輸出一律統一語言」。

### 2.2 問題類型二：內部內容外洩（開發備註、JSON、系統訊息跑到畫面上）
### 2.2.1 問題背景

在 AI Studio 的一個 Prompt 裡，通常同時塞入：

JSON schema

開發註解

系統行為說明

使用者實際輸入的考卷或筆記

如果沒有額外限制，模型會把這些「內部說明」當成可用素材，
直接寫進學生或老師看到的欄位裡。

### 2.2.2 風險

畫面上可能出現：「This is an excellent JSON schema…」之類的句子。

UI 會看起來像 Debug 工具，而不是給老師帶回教室用的系統。

對未來導入正式系統，也會暴露太多內部細節。

### 2.2.3 Prompt 設計原則

在 System Prompt 中，把「什麼不能出現在 UI」講清楚：
```text
You are generating content for teachers and students.

Never mention prompts, JSON schemas, developer notes, or system messages
in any user-facing field.

Do not output JSON, code blocks, or backticks in questions,
explanations, summaries, or homework items.
```

再搭配欄位角色說明，讓模型知道每個欄位只能擺什麼：
```text
The "question" field contains only the problem statement for the student.

The "explanation" field contains only the explanation for the student,
with no internal comments or meta text.

The "teacher_summary" field is a short paragraph written for teachers only.

```
核心概念：用文字把「內部空間」與「使用者空間」切開，
讓模型知道哪些內容是「只能讀、不能說」。

### 2.3 問題類型三：內容過長與欄位混用（一個欄位塞太多資訊）
### 2.3.1 問題背景

模型常常想「一次幫你想到所有需求」，於是：
```
同一個欄位裡塞滿題目全文、解題步驟、教學建議、延伸練習等等。

對 AI 來說是「周到」，對 UI 來說就是「爆版」。

在手機上尤其明顯，一塊卡片就捲不完，老師很難快讀。
```

### 2.3.2 風險

老師為了看一題解析要滑很久。

重要訊息被淹沒，無法快速看出「這個學生最需要補什麼」。

之後要改成正式前端或 API 時，還要再拆一次資料結構。

### 2.3.3 Prompt 設計原則

在 Prompt 裡限制長度與內容種類：

Limit each explanation to at most 3 short sentences:
(1) what went wrong,
(2) the correct idea,
(3) a short tip.

Do not restate the full problem text inside the explanation.
Do not include teacher-focused advice inside student explanations.


並為教師摘要欄位定義單獨用途：
```
In "teacher_summary", write at most 3 bullet points focusing on:
- main conceptual gaps,
- patterns across questions,
- suggestions for follow-up teaching.

Do not repeat the full exam text here.
```

重點是每個欄位只負責一種「閱讀任務」，
把「對學生說」與「對老師說」完全分開。

### 2.4 問題類型四：功能範圍模糊（MVP 只做一位學生，UI 卻像完整 LMS）
### 2.4.1 問題背景

在構思時，很容易同時想到：

班級儀表板

多次測驗的趨勢圖

自動依學習落差分組

學生登入與權限管理

如果沒有收斂，AI Studio 第一版 Prototype 就會什麼都沾到一點，但每個都很淺。

### 2.4.2 風險

Kaggle 評審在 2 分鐘 Demo 裡抓不到主軸。

老師實際使用時，也看不出這工具「第一天」能幫什麼忙。

技術上每一塊都做不深，很難說服任何一方繼續投入。

### 2.4.3 Prompt 設計原則

把 MVP 範圍直接寫死在 System Prompt：
```text
This is an MVP focused on analyzing ONE student's exam or notes at a time.

Do not design or describe features for:
- full LMS,
- student login,
- class dashboards,
- long-term progress tracking.

Keep all UI and explanations focused on a single student's learning gaps
in one exam session.
```

這樣可以：

逼迫自己與模型專注在「單一學生」的體驗。

把班級與長期追蹤留到下一個模組或下一輪實驗再做。

### 2.5 問題類型五：範例資料與真實資料混用（Sample Data 汙染輸出）
### 2.5.1 問題背景

為了讓 Demo 好看，Prompt 裡通常會放一些「示範用考題」與「假學生」。
如果沒有清楚標示，模型可能：

把範例題目視為真實輸入的一部分。

在輸出中混入範例名字或學校。

### 2.5.2 風險

Demo 畫面出現並未在實際上傳考卷中出現的題目或學校名稱。

對未來導入真實學校系統，會造成資料來源與隱私邊界模糊。

2.5.3 Prompt 設計原則

在 Prompt 裡明確標註「哪些欄位才是真實輸入」：
```
Treat the "uploaded_exam_text" and "uploaded_notes" fields
as the ONLY source of real student content.

Any examples inside this prompt are for your understanding only
and must NEVER appear in the output.

Do not invent real student names, IDs, school names, or contact information.
Use generic labels like "the student" or "this learner".
```

核心概念：

真實學生資料只有一條管道進來，其他都視為「說明用 Fake Data」。

模型不得在輸出中引用任何 Prompt 內的範例名字或學校。

## 3. GapLens 實際流程：從單一學生到差異化作業

在套用上述五大原則後，GapLens 被收斂成一條清楚、可 Demo 的流程。

### 3.1 輸入階段（Input）

使用者角色：老師、補救教學者、課後輔導老師。

操作步驟：

在 AI Studio App 中選擇「Exam Mode」。

上傳一位學生的一份考卷（照片、掃描檔或文字）。

視需要輸入簡短背景（年級、科目、單元）。

### 3.2 分析階段（Analysis）

Gemini 3 Pro 的任務：

逐題讀取題目與學生作答內容。

判斷正確與否。

對錯誤題目標記錯誤類型：Conceptual / Procedural / Careless。

推論可能缺乏的概念、技能或策略。

統整成一份「學生學習落差摘要」。

輸出包含：

Student Summary：一小段說明學生目前主要卡關點。

Error Breakdown：逐題錯誤類型、關鍵概念、簡短說明。

### 3.3 差異化作業生成（Personal Homework Pack）

在掌握錯誤分布後，系統會：

針對最關鍵的 2–3 個概念，產生 3–5 題練習題。

每題包含：題目文字、提示（Hint）、正確解答、1–3 句短解析。

設計原則：

不試圖重教整個章節，只補關鍵缺口。

題數控制在老師與學生一節課或一晚可以消化的範圍。

解析避免再度灌輸過多資訊，維持可讀性。

### 3.4 教師摘要與後續建議（Teacher Summary）

額外產出 Teacher Summary，給老師快速判讀本次結果：

三種類型錯誤的大致比例。

可在課堂上重新講解或強調的重點。

未來若有班級層級資料，可進一步建議小組安排或複習順序。

在 AI Studio Prototype 階段，UI 設計重點是：

單一學生路徑要走得完：上傳 → 分析 → 看結果 → 拿到作業。

老師端畫面盡量在 1–2 個螢幕內看懂重點，不需要大量滾動。

## 4. GapLens 在 R0 / 工作流中的位置

從整個系列來看，GapLens 的位置與關聯如下：

與第 1 篇（R0 / 方法論）的關係

GapLens 被視為一個獨立模組，例如 M04_Education_GapLens。

這個模組掛在共用的 Core Kernel 上（例如使用者、帳號、資產等）。

Schema、欄位、模組邊界與對 Core 的存取權限，由 R0 在 SSOT 中維護。

與第 2 篇（溝通地圖）的關係

主要利害關係人是老師、補教老師、教育研究者，大致落在 L3–L5。

溝通時重點放在教學工作流程，而不是模型內部架構。

與第 3 篇（MVP → 上線工作流）的關係

題目：Education / Learning Loss。

MVP 範圍：單一學生的 Exam Mode。

Prototype：Google AI Studio + Gemini 3 Pro。

Demo：最長 2 分鐘的影片，說清楚故事線與實際操作。

提交：Kaggle Writeup + App 連結 + 影片連結。

## 5. 黑客松結束後的「剩餘價值」

即使黑客松結束、GapLens 沒有立刻商轉，仍然有三種明確價值：

AI Orchestrator 作品集案例

展現如何在一週內完成：題目選擇 → MVP 定義 → Prompt 設計 → AI Studio Prototype → Demo → 提交。

教育科技討論的起點

老師、學校、教育研究者可以用 GapLens 當範例，討論：

哪些工作可交給 AI？

哪些判斷必須保留給老師？

如何在不增加老師負擔的前提下，把「看懂落差」這一步自動化。

AI Studio / Gemini Prompt 設計範例

本文整理的五大問題類型與對應原則，可以直接複用到其他教育應用：
作答分析、批改輔助、作文回饋、口說練習等等。

## 6. 後續延伸方向與相關篇章

如果未來要把 GapLens 從黑客松作品升級為正式產品，可能的延伸包括：

與真實成績系統與題庫整合，支援多次測驗與課程結構。

加上班級層級視角與時間序列分析，協助學校做系統性決策。

將差異化作業與既有 LMS / 作業系統整合，讓老師不用多開一個介面。

不論怎麼延伸，仍然會依賴本系列的核心原則：

用 R0 / SSOT / Core + Modules 控制系統複雜度。

用 AI 調度串起多工具，而不是把所有責任交給單一模型。

在每次實驗結束前，優先回收可重用的架構與方法，再決定是否繼續投資該產品。

相關篇章：

第 1 篇：01-r0-ai-orchestrator-methodology-zh-TW.md

第 2 篇：02-ai-orchestrator-communication-map-zh-TW.md

第 3 篇：03-ai-orchestrator-mvp-launch-workflow-zh-TW.md
