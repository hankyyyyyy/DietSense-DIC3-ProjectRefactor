# DietSense 專案核心架構：A to F 摘要、5 Cores、3 Trade-offs 與 10 Details

---

## 一、A to F 六段式研究摘要（Abstract Story）

### A. Attention Getter（引題與研究動機）
全球肥胖與過重人口持續攀升，世界衛生組織（WHO, 2024/2025）指出全球已有超過 26 億名成人過重，其中 9.4 億人屬肥胖，相關代謝慢性病佔全球死因之 74%。在臺灣，衛生福利部國民健康署最新調查顯示，18 歲以上成人過重與肥胖比率已高達 47.9%，年健保相關支出逾新台幣 500 億元。研究證實持續進行飲食熱量自我監測（Self-monitoring）可提升減重成功率達 2 倍，然而 SMARTER 臨床試驗等研究指出，高達 85% 的使用者在 30 天內即放棄記錄。對臺灣民眾而言，「一碗加蛋牛肉麵、半碗湯、一份燙青菜」等複合外食料理，要求手動秤重並拆解食材欄位，認知負擔極高且難以長久維持。

### B. But（現有挑戰與 Research Gap）
現有飲食追蹤工具存在三大技術與行為瓶頸：
1. **手動搜尋與記錄負擔過重**：如 MyFitnessPal、Cronometer 等主流工具，仰賴龐大資料庫中精確關鍵字手動匹配與份量輸入，缺乏在地食物（如夜市小吃、混合羹湯）語義理解能力。
2. **自報飲食攝取誤差巨大**：自我回報熱量之系統性低估誤差常達 18%～54%，隱藏油脂、湯底份量模糊與複合調味擴大了數值估算的不確定性。
3. **通用 LLM 缺乏資料錨定易生幻覺**：直接詢問未經調校的通用語言模型食物熱量時，其平均絕對百分比誤差（MAPE）高達 35.8%，重量估算誤差達 36.3%，且易產生未經醫學驗證的極端節食建議。

### C. Cure（解藥：提出 DietSense Domain AI Agent）
為突破上述瓶頸，本研究提出 **DietSense**——一套**以自然語言驅動、資料錨定（Data-Grounded）且具備安全守門機制之領域智慧代理人（Domain AI Agent）**。其核心設計原則為：
> **「LLM 負責意圖協調與對話互動，領域工具負責客觀取數與精確計算，官方醫學指引負責安全守門。」**

系統透過白名單 Function Calling 串聯多來源可信資料庫與代謝公式，以「熱量信賴區間、信心評分與透明資料來源」取代虛假精確的單一數值。

### D. Development（方法與系統架構）
DietSense 採用模組化八大 Agent 架構（對應規範 ①～⑧）：
1. **輸入與語音處理**：整合 Whisper 語音轉文字、繁體中文語意正規化與四類意圖分流（`log_food`, `query_nutrition`, `analyze_trends`, `recommend_diet`）。
2. **多來源資料錨定（Hybrid RAG）**：結合 USDA FoodData Central（90 萬筆）、臺灣食品成分資料庫，以及自建之「臺灣在地外食向量資料庫（ChromaDB）」。
3. **Clarification Loop（主動追問機制）**：當料理實體、份量（碗/盤/份）或烹調方式之檢索信心度低於 80% 時，主動向使用者發起多輪對話追問澄清。
4. **確定性計算與個人化代謝引擎**：採用 Mifflin-St Jeor 公式精確估算基礎代謝率（BMR）與每日總消耗熱量（TDEE），並依衛福部 AMDR 規範計算巨量營養素比例。
5. **不確定性量化與安全防護層**：傳遞誤差範圍輸出信賴區間；設定每日攝取量 $< 1,200\text{ kcal}$ 熔斷警告，並自動攔截極端節食與飲食失調（Eating Disorder）語句。

### E. Experiments（評估規劃與驗證設計）
本專案為提案階段，規劃以嚴謹客觀指標驗證（非虛構已完成數據）：
1. **標註資料集**：建構 500 筆涵蓋夜市小吃、便當、複合麵食與手搖飲料的真實繁體中文外食語料集，標註標準熱量與巨量營養素範圍。
2. **基準對比（Baseline）**：以市場龍頭 MyFitnessPal 手動記錄為對照組。
3. **使用者研究（User Study）**：招募 20 名受試者進行為期 7 天的真實世界飲食追蹤實驗，透過系統可用性量表（SUS, System Usability Scale）與記錄耗時評估互動體驗。

### F. Findings（預期產出與成果目標）
本專案預期達成之驗證指標與學術貢獻：
* **準確性指標**：在真實外食情境中，熱量估算在 $\pm 20\%$ 容許誤差內之達標率預期高於 **85%**。
* **使用效率**：相較於傳統手動搜尋輸入，飲食記錄時間預期大幅縮短 **60%**。
* **可用性評價**：系統可用性評分 SUS 預期達成 **$> 70$ 分**（達 Good 標準）。
* **學術與實用價值**：展示結合 LLM 協調、在地 RAG 與嚴謹安全邊界的 Domain Agent 如何有效解決自報飲食監測中「高流失、高認知負擔、高估算誤差」的長期公衛痛點。

---

## 二、IEEE Transactions Systems Journal Style Abstract

**Title**—DietSense: A Data-Grounded and Safety-Aware Conversational AI Agent for Personalized Dietary Assessment and Weight Management

**Abstract**—The increasing prevalence of overweight and obesity, affecting an estimated 2.6 billion adults worldwide in 2024, including 940 million individuals living with obesity, underscores the urgent need for sustainable dietary self-monitoring. However, conventional dietary tracking applications impose substantial cognitive burdens, requiring manual food identification, multi-step ingredient search, and portion weight estimation. Self-reported dietary intake errors ranging from 18% to 54% further degrade assessment reliability, particularly for culturally specific mixed dishes. Although large language models (LLMs) enable fluent natural-language interaction, their direct application to nutritional estimation exhibits hallucinated values (caloric MAPE ≈ 35.8%), inconsistent calculations, and unregulated recommendations. To resolve these limitations, this paper proposes **DietSense**, a data-grounded, safety-aware conversational AI agent tailored to traditional Chinese dietary contexts. DietSense establishes a tool-oriented architecture wherein an LLM acts strictly as a reasoning planner coordinating information retrieval through whitelisted function calls, while numerical calculations are delegated to deterministic analytical engines. The framework integrates retrieval-augmented generation (RAG) across the USDA FoodData Central, the Taiwan Food Composition Database, and a specialized vector database of Taiwanese cuisine. Its eight-module architecture features an automated clarification loop that resolves ambiguity in portion sizes and cooking methods whenever confidence drops below 80%. Caloric requirements are calibrated using the Mifflin–St Jeor equation and total daily energy expenditure (TDEE). The evaluation protocol utilizes a curated benchmark of 500 annotated Taiwanese meal descriptions and a planned 7-day user study (N=20). DietSense targets an estimation accuracy exceeding 85% within a ±20% caloric margin, a 60% reduction in logging time relative to manual entry, and a System Usability Scale score exceeding 70. By integrating bounded language reasoning with verifiable domain services, DietSense transitions digital nutrition tracking from burdensome manual logging to transparent, context-aware, and safety-governed health management.

**Index Terms**—Conversational AI, domain-specific AI agents, dietary assessment, large language models, retrieval-augmented generation, uncertainty quantification, personalized weight management, safety-aware systems.

---

## 三、5 Cores（五大核心思維模式）

依據營養學、行為科學與 AI Agent 系統設計共識，提煉五大核心思維：

1. **能量平衡模型（Energy Balance / CICO Principle）**
   * *核心含意*：體重增減遵循熱力學第一定律（攝入熱量 vs. 消耗熱量）。
   * *系統設計落實*：系統以 Mifflin-St Jeor 公式計算基礎代謝率（BMR），結合日常活動係數計算 TDEE，作為個人化動態天花板與基準線，杜絕缺乏能量平衡概念的盲目節食。
2. **巨量營養素比例與代謝效應（Macronutrient Composition & AMDR）**
   * *核心含意*：熱量並非唯一維度，碳水化合物（$4\text{ kcal/g}$）、蛋白質（$4\text{ kcal/g}$）與脂肪（$9\text{ kcal/g}$）的供能比例與食物熱效應（TEF）直接影響飽足感、肌肉合成與代謝健康。
   * *系統設計落實*：落實臺灣衛福部可接受巨量營養素分佈範圍（AMDR），在報告中提供碳水 50-65%、蛋白質 10-20%、脂肪 20-30% 之結構化分解，而非單一只看熱量。
3. **份量估算的不確定性量化（Portion Uncertainty Quantification）**
   * *核心含意*：日常外食的份量（「一碗」、「半份」）本質上存在測量噪音與主觀偏差，追求虛假的單點精確（Point Estimate, 如宣稱 634.2 kcal）反而是不科學且誤導使用者的。
   * *系統設計落實*：計算管線輸出信賴區間（Confidence Interval, 如 $600\sim 680\text{ kcal}$），揭露數據來源與假設，並計算信心分數（Confidence Score）。
4. **個體生理與情境差異（Individual Physiological Variation）**
   * *核心含意*：相同的飲食攝入在不同年齡、性別、BMI、運動頻率與病史個體上會產生截然不同的代謝反應。
   * *系統設計落實*：建立個人化 Profile 模組，依使用者生理參數動態更新個人化熱量赤字建議（每日建議赤字 300~500 kcal），避免一刀切的通用處方。
5. **行為情境意識與長期遵從性（Behavioral Context Awareness & Adherence）**
   * *核心含意*：減重失敗的核心瓶頸在於使用者流失與認知疲倦，極端繁瑣的記錄機制必然導致失敗。
   * *系統設計落實*：以自然語言與語音輸入作為第一接觸面，將單次記錄負擔降至 15 秒內，透過主動追問與正向回饋機制提升長期遵從率。

---

## 四、3 Trade-offs（三大專業爭論與架構權衡）

| 爭論面向 | 觀點 A (Pros & Cons) | 觀點 B (Pros & Cons) | 領域現有共識 | DietSense 架構抉擇與落地 |
| :--- | :--- | :--- | :--- | :--- |
| **Trade-off 1：<br/>卡路里數量 vs. 食物品質** | **嚴格熱量赤字說（CICO）**：只要總熱量低於消耗即會減重；缺點是忽視胰島素反應、微量營養素與飢餓感。 | **荷爾蒙／低升糖說**：強調原型食物、低 GI、抗發炎；缺點是若總熱量過剩仍無法達成減重。 | 能量赤字是減重之**必要條件**，而食物品質與巨量比例則是長期維持與健康的**充分條件**。 | **雙軌制呈現**：同時輸出熱量平衡區間（對比 TDEE）與巨量營養品質雷達圖，並在建議模組提出低 GI/高蛋白食材替代建議。 |
| **Trade-off 2：<br/>對話估算 vs. 硬體秤重/3D影像** | **純自然語言對話／LLM 估算**：極度便利、門檻最低、支援複雜烹調情境；缺點是精準度受限於使用者回報品質。 | **精確秤重／3D 深度視覺硬體**：測量物理精確度高；缺點是使用者出門在外無法隨身攜帶秤，流失率極高。 | 在公共衛生與日常自主管理中，**「能長期持續的良好估算」遠優於「只能維持三天的完美測量」**。 | **對話為主＋Clarification Loop**：日常採自然語言對話，當關鍵份量模糊時啟動主動追問（如「麵是大碗小碗？」），在便利性與精確度取得最佳折衷。 |
| **Trade-off 3：<br/>公共衛生通用指引 vs. 高度精準個人化** | **官方公衛常規飲食指南**：經過大規模流行病學實證、具法律與安全性保護；缺點是無法針對個體基因或代謝微調。 | **基因/腸道菌多體學個人化**：理論上最精準；缺點是商業成本極高、醫療診斷邊界模糊且可能引發醫療糾紛。 | 數位健康工具必須以**「安全性第一」**為底線，不得逾越非醫療軟體的合規邊界。 | **安全性熔斷＋在地指南**：以衛福部國人每日飲食指南為核心基線，設定每日熱量不得低於 1200 kcal 之硬性安全門檻，特殊族群（孕婦、糖尿病）強制引導就醫。 |

---

## 五、10 Details（十大技術實作關鍵細節）

1. **Whisper 語音轉文字與繁簡在地實體正規化**：將口語化語音轉為文字，透過自建正則與辭典將臺灣習慣用語（如「滷肉飯」、「半糖去冰」、「排骨便當」）統一編碼。
2. **四態意圖分流路由（Intent Router）**：設計高準確意圖分類器，將輸入嚴格分流為飲食記錄（`log_food`）、營養查詢（`query_nutrition`）、趨勢分析（`analyze_trends`）與建議回饋（`recommend_diet`）。
3. **雙層混合 RAG 檢索機制**：先對「臺灣在地外食向量庫」進行稠密向量檢索（Dense Vector Search），若未命中則回退至「臺灣食品成分資料庫」與 USDA API 進行關鍵字精確匹配。
4. **Clarification Loop 信心門檻（Confidence Threshold = 80%）**：當檢索相似度或份量抽取信心度 $< 0.80$ 時，觸發追問分支，主動要求使用者補充模糊資訊（例如湯品飲用量、烹調方式）。
5. **Mifflin-St Jeor 確定性 TDEE 運算引擎**：完全由 Python 確定性數學函式執行能量計算，禁止由 LLM 自行加減乘除，公式嚴格遵循：
   $$\text{BMR} = 10 \times \text{weight(kg)} + 6.25 \times \text{height(cm)} - 5 \times \text{age(y)} + s \quad (s = +5 \text{ for male}, -161 \text{ for female})$$
6. **巨量營養素熱量等價轉換機制**：採用國際標準係數（碳水 $4\text{ kcal/g}$、蛋白質 $4\text{ kcal/g}$、脂肪 $9\text{ kcal/g}$），雙向驗證總熱量與各項總和之一致性。
7. **不確定性區間動態傳遞**：為每個食物實體標記份量不確定係數（如小吃估計誤差 $\pm 15\%$），在多食物加總時透過均方根或誤差傳遞公式輸出信賴區間（例如 $650 \pm 80\text{ kcal}$）。
8. **每日熱量 $< 1,200\text{ kcal}$ 安全熔斷攔截（Safety Guardrail）**：當使用者設定或每日目標低於 1200 kcal 時，系統強制拒絕產生過度節食計劃，並跳出健康危害警告。
9. **飲食失調（Eating Disorder）與極端詞彙守門員**：內建心理健康敏感詞庫（如催吐、絕食、強效減肥藥），偵測到異常偏激行為時立即終止減重引導，並提示心理與營養諮詢專線。
10. **多輪對話狀態機與非侵入式歷史快照**：以結構化 Session State 記錄當日餐別（早/午/晚/點心），使用者可隨時用自然語言追加修改（例如「剛才那杯奶茶其實是微糖」），由 Agent 精確覆寫單項數值。

---

## 六、2020 A-Z SOP 全字母對齊映射總表

| 字母 | A2Z 標準要素名稱 | DietSense 專案具體落地與章節映射 |
| :---: | :--- | :--- |
| **A** | **Attention Getter** | 引用 WHO 2025 全球 26 億過重/9.4 億肥胖數據、臺灣成人過重率 47.9% 及 500 億健保負擔。 |
| **B** | **But (Challenge)** | 指出現有 App 手動輸入繁瑣、在地食物匱乏、自報誤差 18%～54%、85% 一個月內放棄。 |
| **C** | **Cure (Solution)** | 提出 DietSense：對話式、資料錨定、安全守門之繁體中文領域 AI Agent。 |
| **D** | **Development (Method)** | 八大模組架構、Whisper + RAG + NER + Mifflin-St Jeor TDEE + 信心度追問迴圈。 |
| **E** | **Experiments (Evaluation)** | 500 筆在地外食標註資料集、對比 MyFitnessPal、20 人 7 天 User Study 與 SUS 評估。 |
| **F** | **Findings (Outcomes)** | 預期 $\pm 20\%$ 誤差內達標率 $> 85\%$、記錄時間縮短 $60\%$、SUS $> 70$ 分。 |
| **G** | **General 現況** | 全球數位健康體重管理市場達 68 億美元，生活型態介入具顯著慢性病預防效益。 |
| **H** | **However 挑戰深入** | 深入剖析四認知障礙：查詢負擔、份量模糊、複合菜餚拆解困難、通用 LLM 幻覺嚴重。 |
| **I** | **In literature 文獻解法** | 梳理文獻歷程：第 1 代手動輸入 $\to$ 第 2 代影像辨識 $\to$ 第 3 代對話式 Agent 與 Tool Use。 |
| **J** | **Judgement (SWOT)** | 系統化評價：拍照難估隱藏油鹽、純 LLM 數值漂移，論證「LLM 協調 + 確定性工具」之必要。 |
| **K** | **Keypoint & Contribution** | 強調四大亮點：自然語言輸入、主動互動、情境感知、大數據驅動智慧。 |
| **L** | **List Organization** | 交代全篇架構：第 2 節相關文獻、第 3 節系統設計、第 4 節實驗設計、第 5 節結論。 |
| **M** | **Methods 文獻分類** | 分類探討：AI 面（LLM, RAG, Food-NER）與 Domain 面（能量平衡, AMDR, 臺灣飲食指引）。 |
| **N** | **New Proposed 創新點** | 提出 Hybrid RAG（整合在地向量庫與官方成分庫）以及動態 Clarification Loop 機制。 |
| **O** | **Organize 文獻總結** | 建立 5 篇跨 2024–2026 文獻摘要比較表（4x5 / Article Summary Table），收斂 Research Gap。 |
| **PA** | **Aim / Statement 目標** | 打造全臺首套兼顧在地語意理解與醫療安全標準之飲食評估對話 Agent。 |
| **PB** | **Based-on / Benefit** | 基於 Mifflin-St Jeor 與衛福部國人每日飲食指南，大幅降低飲食記錄心理抗拒。 |
| **PC** | **Cure / Cause 有用原因** | 解決「黑盒大模型不可信」問題，將資料取得與數值計算交由可驗證之確定性工具。 |
| **PD** | **Design 架構轉移** | 從傳統「單純問答式 Chatbot」轉型為具有「規劃、工具調用、追問與自我修正」的 Agent。 |
| **PE** | **Paper Element (模組)** | 完整對應規範 ① 用戶情境、② 前處理、③ Planner、④ 檢索、⑤ 分析、⑥ 知識、⑦ 決策、⑧ 互動。 |
| **PF** | **Formulation / Foundation** | 建立熱量計算公式 $E = \sum (\text{weight}_i \times \text{kcal}_{100\text{g},i} / 100)$ 與 TDEE 活動量乘數方程式。 |
| **PG** | **Graph 架構圖說明** | 提供清晰八大模組資料流架構圖與模組關聯拓撲。 |
| **PH** | **How 設計如何達成** | 透過 LangChain / Function Calling 路由、白名單工具隔離與非同步對話管理。 |
| **PI** | **Implementation / Pseudo** | 提供 Algorithm 1 具行號之核心演算法（含意圖、檢索、追問、計算與安全檢驗）。 |
| **PJ** | **Jump to an Example** | 以真實情境「一碗加蛋牛肉麵，湯喝一半」示範實體拆解、模糊追問到產出區間全流程。 |
| **PK** | **Key Points Contribution** | 貢獻繁中外食對話 Agent 架構、500 筆小吃資料集、透明區間評估標準。 |
| **PL** | **Later Simulation** | 規劃離線 Benchmark 測試（評估檢索命中率）與線上真實用戶實驗流程。 |
| **PM** | **Math / Uncertainty Proof** | 數學推導份量模糊度在熱量乘積中的不確定性傳遞模型（信賴區間邊界計算）。 |
| **Q** | **Quality & Quantity 指標** | 定義質化（SUS 可用性量表）與量化指標（$\pm 10\%, \pm 20\%$ 誤差率、記錄耗時秒數）。 |
| **R** | **Related Method Compare** | 嚴格選定 Baseline：MyFitnessPal（手動）、GPT-4o Direct Prompting（純對話無工具）。 |
| **S** | **Simulation Setup** | 20 名介於 20~50 歲外食受試者、7 天跨餐期紀錄、受控環境與自由外食雙重測試。 |
| **T** | **Tuning Parameters** | 調整 Clarification 觸發門檻（測試 0.70, 0.80, 0.90）、向量檢索 Top-K 與 Temperature。 |
| **U** | **Useful Results (預期)** | 預期熱量偏差顯著低於通用 LLM，且記錄耗時從 120 秒降至 45 秒以內。 |
| **V** | **Verify Effectiveness** | 驗證「資料錨定 + 主動追問」架構能否在真實雜訊環境下保持估算穩健性。 |
| **W** | **What Proposed 回顧** | 總結 DietSense 如何克服手動記錄疲乏與黑盒模型幻覺的雙重挑戰。 |
| **X** | **eXcel 專精情境** | 特別專精於臺灣夜市、傳統小吃、便當與手搖飲料等非標準化外食場景。 |
| **Y** | **Yields 產出成果** | 產出開源外食評估 Benchmark、Agent 實作原型與高滿意度評估數據。 |
| **Z** | **Zen 結論與未來展望** | 結論強調「可信任 AI」在預防醫學中的關鍵價值；未來將整合穿戴裝置與連續血糖監測（CGM）。 |

