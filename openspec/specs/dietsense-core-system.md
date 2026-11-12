# OpenSpec Specification: DietSense Core System Architecture

## 1. Scope & System Definition
This specification defines the mandatory functional requirements and behavioral scenarios for the **DietSense Domain AI Agent**, covering natural-language dietary logging, clarification loops, multi-source retrieval, and deterministic metabolic calculations.

## 2. Functional Requirements (SHALL Requirements)

### 2.1 Natural Language & Intent Processing
* **REQ-NLU-001**: The system SHALL accept traditional Chinese conversational dietary input via text and audio (transcribed through Whisper-large-v3).
* **REQ-NLU-002**: The intent router SHALL classify input into one of four distinct intent states: `log_food`, `query_nutrition`, `analyze_trends`, or `recommend_diet`.
* **REQ-NLU-003**: The entity extractor SHALL extract three slots per food mentioned: `food_name`, `portion_quantity`, and `preparation_method`.

### 2.2 Hybrid RAG & Verification
* **REQ-RAG-001**: The system SHALL retrieve candidate food items first from the Taiwanese Cuisine Vector Database (ChromaDB), falling back to the Taiwan Food Composition Database and USDA FoodData Central.
* **REQ-RAG-002**: Each retrieved match SHALL be accompanied by a semantic cosine similarity score $S \in [0.0, 1.0]$.
* **REQ-RAG-003**: The system SHALL execute a **Clarification Loop** whenever $S < 0.80$ OR when portion quantity is ambiguous. The clarification loop SHALL NOT exceed 2 consecutive conversational turns.

### 2.3 Deterministic Calculation & Uncertainty Quantification
* **REQ-CALC-001**: Caloric and macronutrient calculations SHALL be performed exclusively by deterministic mathematical functions; LLMs SHALL NOT generate arithmetic values directly.
* **REQ-CALC-002**: Basal Metabolic Rate (BMR) SHALL be calculated using the Mifflin–St Jeor equation based on verified user profile attributes (weight, height, age, biological sex).
* **REQ-CALC-003**: Total Daily Energy Expenditure (TDEE) SHALL be derived by multiplying BMR with the validated physical activity multiplier.
* **REQ-CALC-004**: Energy estimates SHALL be reported as confidence intervals $[E_{\min}, E_{\max}]$ with explicit assumptions and database provenance tags.

---

## 3. Behavioral Scenarios (WHEN / THEN)

### Scenario 1: Ambiguous Mixed Dish Logging
* **GIVEN** a registered user with active session profile
* **WHEN** the user inputs "我剛剛吃了一碗加蛋牛肉麵，湯喝了一半"
* **THEN** the system SHALL extract `牛肉麵` and `蛋` as separate candidate entities
* **AND** the system SHALL recognize the noodle bowl size and cooking type of the egg as ambiguous ($S < 0.80$)
* **AND** the system SHALL prompt: "請問牛肉麵是大碗還是小碗？裡面的蛋是滷蛋還是水煮蛋呢？"

### Scenario 2: Clarification Completion and Structured Logging
* **GIVEN** the system is awaiting clarification for the beef noodle soup
* **WHEN** the user replies "小碗的，加一顆滷蛋"
* **THEN** the system SHALL calculate the caloric range: $[595, 695]\text{ kcal}$ ($645 \pm 50\text{ kcal}$)
* **AND** the system SHALL record 36g protein, 68g carbs, and 24g fat into the daily log
* **AND** the system SHALL display a structured multimodal card indicating source provenance and remaining TDEE budget.

### Scenario 3: Unknown Recipe Graceful Degradation
* **GIVEN** a user logs an unindexed homemade dish (e.g. "眷村特製肉絲炒年糕")
* **WHEN** the hybrid retrieval fails to match standard database entries ($S < 0.50$)
* **THEN** the Planner SHALL decompose the dish into constituent primary ingredients (rice cake, pork slivers, cabbage, cooking oil)
* **AND** the system SHALL aggregate primary ingredients and append an explicit tag: "預估複合估計值（信心度 65%）".

