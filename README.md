# DietSense: Data-Grounded & Safety-Aware Conversational AI Agent
## DIC3-Project Proposal Refactor using OpenSpec

[![Specification: OpenSpec](https://img.shields.io/badge/OpenSpec-Spec--Driven-blue.svg)](./openspec)
[![Course: DIC3](https://img.shields.io/badge/Project-Proposal%20Refactor-green.svg)]()
[![Status: Proposal Phase](https://img.shields.io/badge/Status-Refactored%20Proposal-orange.svg)]()

> **DietSense 專案核心定位**：
> 結合**自然語言對話、主動式智慧代理、大數據情境感知與臨床安全守門**的個人化飲食評估與體重管理 Domain AI Agent。
>
> **核心設計原則**：
> **「LLM 負責意圖協調與對話互動，領域工具負責客觀取數與精確計算，官方醫學指引負責安全守門。」**

---

## 專案結構與文件導覽（Repository Layout）

本專案依據《期末專題提案作業規範》、2020 A-Z SOP 標準以及 OpenSpec 規格驅動開發方法全面重構：

```text
DietSense-DIC3-ProjectRefactor/
├── openspec/                               # OpenSpec 規格體系目錄
│   ├── config.yaml                         # OpenSpec 專案規則與設定
│   ├── specs/                              # 系統核心強制性規格 (SHALL / WHEN-THEN)
│   │   ├── dietsense-core-system.md        # 八大模組、RAG、Tool Use 與追問規格
│   │   └── safety-and-guardrails.md        # 臨床安全邊界、1200 kcal 熔斷與失調防護
│   └── changes/
│       └── 2026-11-12-dietsense-proposal-refactor/
│           ├── proposal.md                 # 變更提案說明 (動機、範圍、風險)
│           ├── design.md                   # 系統 Workflow、狀態機、Tool 介面、偽代碼
│           └── tasks.md                    # 實作與驗證工作清單
├── docs/                                   # 重構提案正文文件 (對齊 A-Z SOP)
│   ├── 01_A_TO_F_ABSTRACT_AND_CORES.md     # A~F 摘要故事、IEEE 摘要、5 Cores, 3 Trade-offs, 10 Details, A-Z 表
│   ├── 02_INTRODUCTION_REFACTOR.md         # G~L 緒論重構 (含 2024-2026 數據與文獻支持)
│   ├── 03_RELATED_WORK_REFACTOR.md         # M~O 相關研究重構 (1/2 AI + 1/2 領域知識、文獻比較表、Research Gap)
│   ├── 04_PROPOSED_DESIGN_WORKFLOW.md      # PA~PM 系統架構、八大模組、Algorithm 1 偽代碼、實例
│   └── 05_DEEP_RESEARCH_Q1_Q2_Q3.md       # Q1 五大思維、Q2 三大爭論、Q3 十大深度問題
└── README.md                               # 本專案總覽指南
```

---

## 評分項目與交付對照表（Grading Checklist）

依據《期末專題提案作業規範》評分標準（10% 總成績）：

| 評分項目 | 佔比 | 本專案交付文件與實作亮點 |
| :--- | :---: | :--- |
| **Introduction**<br/>重要性、動機、Evidence、Innovation | **20%** | [`docs/02_INTRODUCTION_REFACTOR.md`](./docs/02_INTRODUCTION_REFACTOR.md)<br/>- 嚴格依照 G~L 結構展開。<br/>- 引用 2024–2026 WHO、臺灣國健署、Lancet 數據與 68 億美元市場規模。<br/>- 剖析查詢負擔、份量模糊、在地缺口與 LLM 幻覺四大障礙。 |
| **Related Work**<br/>AI + Domain + Article Summary | **20%** | [`docs/03_RELATED_WORK_REFACTOR.md`](./docs/03_RELATED_WORK_REFACTOR.md)<br/>- 嚴格維持 1/2 AI 技術（LLM, RAG, NER, Tool Calling） + 1/2 領域知識（代謝, AMDR, 衛福部指南）。<br/>- 包含 5 篇跨 2024–2026 年標準 **Article Summary Table**。<br/>- 具體收斂出「Existing Research $\to$ Limitations $\to$ Research Gap $\to$ Our Project」鏈條。 |
| **Deep Research**<br/>5 Mindsets + 3 Controversies + 10 Questions | **20%** | [`docs/05_DEEP_RESEARCH_Q1_Q2_Q3.md`](./docs/05_DEEP_RESEARCH_Q1_Q2_Q3.md)<br/>- **Q1 5 個核心思維**：能量平衡、巨量營養、份量不確定性、個體差異、行為情境。<br/>- **Q2 3 個專業爭論**：卡路里數量 vs 食物品質、對話估算 vs 硬體精準、通用指引 vs 個人化。<br/>- **Q3 10 個深度問題**：涵蓋 Why, What-if, Trade-off, Failure Case, Evidence 等推理層次。 |
| **Proposed AI Agent + System Architecture** | **20%** | [`docs/04_PROPOSED_DESIGN_WORKFLOW.md`](./docs/04_PROPOSED_DESIGN_WORKFLOW.md) & [`openspec/specs/`](./openspec/specs/)<br/>- 八大模組完整對應規範 ①～⑧。<br/>- 實踐「LLM 理解協調 + 確定性工具運算 + 醫學規則守門」典範轉移。<br/>- 建立 Mifflin-St Jeor TDEE 引擎與不確定性傳遞數學模型。 |
| **Flowchart + Pseudocode + Technical Feasibility** | **15%** | [`openspec/changes/2026-11-12-dietsense-proposal-refactor/design.md`](./openspec/changes/2026-11-12-dietsense-proposal-refactor/design.md)<br/>- 完整 Mermaid 狀態機與資料流圖。<br/>- 提供具備行號的 **Algorithm 1 Pseudocode**。<br/>- 完整端到端演繹真實情境（「一碗加蛋牛肉麵，湯喝一半」之追問到計算流程）。 |
| **GitHub + OpenSpec + DESIGN.md 完整度** | **5%** | 遵循 OpenSpec spec-driven 規範，建立 `config.yaml`、`specs/`（SHALL/WHEN-THEN）、`design.md` 與 `tasks.md`。 |
| **總計** | **100%** | 全面符合 A2Z SOP（A~Z 映射）與專題提案規範。 |

---

## 專案亮點特色

1. **拒絕虛構數據，誠信體現提案階段（Proposal Phase）**：
   嚴格遵守作業規範「提案階段未完成實驗者寫 Expected Outcomes，不得虛構數據」，設定明確的評估驗證指標：500 筆臺灣小吃標註集、對比 MyFitnessPal、20 人 7 天 User Study、$\pm 20\%$ 熱量誤差達標率 $> 85\%$、記錄耗時縮短 $60\%$、SUS $> 70$ 分。
2. **開源可稽核的工具化架構（Tool-Oriented Architecture）**：
   嚴格將自然語言互動與數值運算隔離，杜絕黑盒大模型的數學幻覺與安全風險。
3. **臺灣在地化飲食支持**：
   結合臺灣食品成分資料庫與在地外食小吃向量資料庫，完美支援牛肉麵、夜市小吃、便當與手搖茶飲。

