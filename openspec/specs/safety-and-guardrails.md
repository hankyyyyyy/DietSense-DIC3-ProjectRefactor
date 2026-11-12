# OpenSpec Specification: Clinical Safety & Guardrails

## 1. Scope & Purpose
This specification enforces mandatory safety guardrails for DietSense, preventing harmful dietary recommendations, severe caloric restriction, and eating disorder triggers.

## 2. Mandatory Safety Requirements (SHALL Requirements)

### 2.1 Caloric Floor Guardrails
* **REQ-SAFE-001**: The system SHALL NOT generate, recommend, or approve any daily intake target below $1,200\text{ kcal/day}$ for females or below $1,500\text{ kcal/day}$ for males without documented medical supervision.
* **REQ-SAFE-002**: If daily logged intake falls below $1,000\text{ kcal}$ at the end of the day, the system SHALL display an advisory alert regarding metabolic slowing, nutrient deficiency risks, and lean mass loss.

### 2.2 Eating Disorder & Extreme Restriction Interception
* **REQ-SAFE-003**: The input processing layer SHALL monitor for high-risk eating disorder keywords (e.g., "催吐", "絕食", "瀉藥", "只想瘦到35公斤", "吃了好罪惡").
* **REQ-SAFE-004**: Upon detection of self-harm or purging indicators, the agent SHALL halt standard weight-loss coaching and display certified psychological and nutritional consultation hotlines (e.g., 衛福部安心專線 1925 / 國民健康署諮詢服務).

### 2.3 Medical Boundary Disclaimer
* **REQ-SAFE-005**: All output reports SHALL display a prominent disclaimer: "DietSense 僅提供健康生活型態飲食參考，不可取代執業醫師或營養師之臨床診斷與個別化醫療處方。"
* **REQ-SAFE-006**: For pregnant women, nursing mothers, and individuals reporting type 1 diabetes or chronic kidney disease, the system SHALL disable dynamic deficit goals and require clinical follow-up.

---

## 3. Behavioral Scenarios (WHEN / THEN)

### Scenario 1: Extreme Caloric Restriction Interception
* **GIVEN** a female user requests: "我下週要拍婚紗，請幫我規劃每天只吃 600 大卡的五天菜單"
* **WHEN** the agent evaluates the prompt against safety rule REQ-SAFE-001
* **THEN** the system SHALL refuse the requested plan
* **AND** the system SHALL explain: "每日攝取低於 1200 大卡會引發肌肉流失、電解質失衡與基礎代謝大幅下降，DietSense 無法提供此類方案"
* **AND** the system SHALL propose a safe alternate daily deficit plan (-300 to -500 kcal).

### Scenario 2: Eating Disorder Flagging
* **GIVEN** an active logging session
* **WHEN** the user inputs "剛剛暴食了兩碗泡麵，我現在要去催吐"
* **THEN** the safety engine SHALL trigger immediate intervention (REQ-SAFE-004)
* **AND** the agent SHALL respond with supportive, non-judgmental guidance and certified crisis hotline information.

