# 系列文第 1 篇：Arin 的 R0 / AI Orchestrator 方法論  
以 SSOT 與 Core Kernel + Modules 設計 AI 工作流

English: [01-r0-ai-orchestrator-methodology.md](./01-r0-ai-orchestrator-methodology.md)  
繁體中文：本篇

---

## 0. 系列地圖：本篇在整體中的位置

本篇是「AI Orchestrator / R0 系列」的第 1 篇，負責打底概念：

- 為何需要 **R0 / AI Orchestrator** 這個角色？  
- 為何要用 **SSOT（Single Source of Truth）** 管理專案知識？  
- 為何推薦用 **Core Kernel + Modules** 來規劃系統架構？  
- L1–L10 在這裡代表的是「工作層級」，不是職稱或 AI 熟練度。

後續篇章的關係：

- 第 2 篇：用 L1–L10 描述「人／團隊使用 AI 的成熟度」，對應不同溝通方式。  
- 第 3 篇：把 R0 方法論實作成一條「MVP → 上線」工作流。  
- 第 4 篇：將這套方法實際套用在 Kaggle 教育案例 GapLens 上。

---

## 1. 我是誰：AI Orchestrator / AI Workflow Architect

在這套方法論中，「我」的定位是：

- **AI Orchestrator / AI Workflow Architect**  
- 核心任務是：  
  - 設計、維護一條由多個 AI 工具、模型與資料源組成的工作流；  
  - 確保這條工作流有清楚定義的 **Single Source of Truth（SSOT）**；  
  - 把人類決策放在對的位置，而不是完全交給 AI 或完全手工。

這個角色不等於「會寫 Prompt 的人」，而比較像是：

- 把 AI 當成「分工的團隊」，  
- 負責定義誰負責什麼、工作交接點在哪裡、以及最後誰對結果負責。

---

## 2. 為什麼需要 SSOT（Single Source of Truth）？

在日常使用 AI 的過程中，很容易出現幾種常見問題：

- 設計決策散落在 Chat / Notion / Google Doc，不易回溯。  
- 不同模型對同一問題給出不一致的回答。  
- 專案換人之後，沒有人說得清楚「現在到底哪個版本才是準的」。

為了避免這種「認知分裂」，我採用 **SSOT** 作為底層原則：

1. **所有長期決策依據與共享事實**，必須註冊在明確結構中  
   - 通常是：  
     - 一份 JSON 狀態  
     - 一組資料庫 schema（Core Kernel + Modules）  
   - 只要牽涉「長期維運」與「多人協作」，都要寫進 SSOT。

2. **AI 不得直接、靜默修改 SSOT**  
   - AI 可以給出「建議變更（diff）」；  
   - 但實際寫入 SSOT 的權限，只放在 R0（人類）或 R0 驗證過的自動流程手上。

3. **SSOT 是人類與 AI 的共同語言**  
   - 所有模組、功能、PRD、程式碼，都要能回溯到同一份 SSOT。  
   - 這也是未來「多 AI 協作」時，非常關鍵的共通協議。

---

## 3. Core Kernel + Modules：系統架構的基本盤

在系統設計上，我將整個產品抽象成兩層：

1. **Core Kernel（核心）**
   - 代表長期穩定、不因某一功能而變動的實體：  
     - 例如：User、Location、Account、Global Settings、Event Log 等。
   - 特性：  
     - 不依賴任何特定模組也能獨立運作；  
     - schema 變動頻率低，需要嚴格審核。

2. **Modules（模組）**
   - 以 `Module_ID` + `table_prefix` 管理的可插拔功能區塊：  
     - 例如：`M02_Subscription`、`M03_CityIntel`、`M04_Education_GapLens`。  
   - 每個模組擁有自己的：  
     - 資料表（以 prefix 區分）  
     - API 介面與授權邏輯  
   - 與 Core 的關係必須透過「明確介面」而不是私下共用 table。

這種設計有幾個好處：

- 可以替換或重寫單一模組，而不影響核心。  
- 不同實驗（例如副業工具、教育案例）可以共用同一套 Core。  
- 很適合多 AI / 多團隊同時協作，每個人只負責自己模組的範圍。

---

## 4. L1–L10：工作層級，而不是職銜

在 R0 方法論裡，我使用一套 L1–L10 來標記「工作本身的抽象層級」，而不是人的資歷。

簡化版對照如下：

| 層級 | 角色／性質                      | 典型任務                                      |
| ---- | ------------------------------- | --------------------------------------------- |
| R0   | Root Orchestrator / SSOT 管理  | 定義 SSOT、Core/Modules 邊界、審核變更 diff   |
| L1   | 問題定義（Problem Framing）    | 釐清目標、限制、成功指標                      |
| L2   | 系統與資料架構（System/Data）  | 設計模組、schema、介面契約                    |
| L3   | 實作（Implementation）         | 產生程式碼、查詢、設定                        |
| L4   | 測試與覆核（QA / Review）      | 測試案例、邊界情境、品質檢查                  |
| L5   | 使用者敘事（UX / Narrative）   | 文案、流程說明、教學引導                      |
| L6   | 分析與觀測（Analytics）        | 事件設計、KPI、實驗結果解讀                  |
| L7   | 產品組合與策略（Strategy）     | 決定做／停／擴張哪些功能                      |
| L8   | 組織與流程（Org / Process）    | 設計團隊如何與 AI 工作流互動                  |
| L9   | 生態系與整合（Ecosystem）      | 外部 API、夥伴系統、治理架構                  |
| L10  | 願景與哲學（Vision）           | 長期願景、價值觀與定位                        |

實務上，R0 要做的事情包括：

- 把一個題目拆解成 L1–L10 的任務組合；  
- 決定哪些層級交給 AI，哪些層級一定要人類主導；  
- 對每一層級設計對應的 Prompt、輸入輸出格式與驗收標準。

---

## 5. R0 的具體職責

在這套方法論中，**R0（Ring-0 / Root Orchestrator）** 的主要責任是：

1. **維護 SSOT 與架構邊界**
   - 管理 `project_meta`、`core_kernel`、`modules_map`、`integration_registry` 等。  
   - 審查每一個 AI 或人類提出的變更，決定是否寫入。

2. **定義模組生命週期**
   - 指定 Module ID 與 table prefix 命名規則。  
   - 定義模組可讀／可寫的資料範圍。  
   - 規劃 checkout / merge 流程（給其他 AI 或協作人員使用）。

3. **產生最小上下文（Context Pack）**
   - 對於每個下游任務，只暴露必要的 Core 資訊（多半唯讀）。  
   - 清楚列出「可以改什麼」與「不能動什麼」。  
   - 減少 AI 因為得到太多資訊而產生不必要的 hallucination。

4. **維持人類在關鍵節點的參與**
   - 重要的是「決定人類要在哪裡出現」：  
     - 題目選定（L1）  
     - 架構確認（L2）  
     - 重大上線與 rollback 決策（L7）  
   - 不是把人類排除，而是讓人類做更有價值的決策。

---

## 6. 一個簡化示例：訂閱管理工具

以「訂閱管理 / 數位資產庫」這個題目為例：

- **Core Kernel**  
  - `users`、`accounts`、`transactions`、`assets` 等長期不變的實體。
- **Modules**  
  - `M02_Subscription`：管理每月訂閱、費用預估。  
  - `M03_AssetLibrary`：整理遊戲、課程等一次性資產。  
  - `M04_BudgetPlanner`：結合收入、支出、訂閱，給出預算建議。

R0 要處理的事情包括：

- 在 SSOT 裡定義每個模組的資料表與對 Core 的依賴。  
- 規範哪些任務可以交給 AI（例如產生 PRD、初版 schema、API 草稿），哪些一定要人工審查（例如實際 schema migration）。  
- 當未來要新增「教育訂閱分析」或「GapLens 類教育模組」時，只需新增模組，不破壞既有架構。

---

## 7. 延伸閱讀與下一步

- 想了解「如何用同一套 L1–L10 模型來判斷人／團隊的 AI 使用成熟度」，可看系列文第 2 篇：  
  - [`02-ai-orchestrator-communication-map-zh-TW.md`](./02-ai-orchestrator-communication-map-zh-TW.md)

- 想直接看「MVP → 上線」的實作流程，可看系列文第 3 篇：  
  - [`03-ai-orchestrator-mvp-launch-workflow-zh-TW.md`](./03-ai-orchestrator-mvp-launch-workflow-zh-TW.md)
