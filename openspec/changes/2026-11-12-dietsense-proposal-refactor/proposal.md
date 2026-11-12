# OpenSpec Change Proposal: DietSense Proposal Refactor (DIC3)

## 1. Problem & Motivation
Traditional dietary self-monitoring software (e.g. MyFitnessPal) suffers from high attrition rates (85% abandonment within 30 days) due to burdensome multi-step ingredient search and manual portion entry. Meanwhile, general LLMs (e.g. raw ChatGPT) hallucinate caloric estimates (MAPE ≈ 35.8%) and lack safety boundaries.

This change refactors the DietSense final project proposal according to the 2020 A-Z SOP and OpenSpec spec-driven standards, establishing a verifiable Domain AI Agent architecture combining Traditional Chinese conversational NLU, local food RAG, deterministic energy calculation, and clinical safety guardrails.

## 2. Proposed Changes & Deliverables
1. **OpenSpec Architecture Foundation**:
   - Project configuration `openspec/config.yaml`.
   - Core specifications `openspec/specs/dietsense-core-system.md` and `safety-and-guardrails.md`.
2. **Structural Synthesis**:
   - Comprehensive A to F narrative abstract and IEEE Systems journal abstract.
   - 5 domain mental models, 3 professional controversies, and 10 in-depth evaluative questions.
3. **Workflow & Design Refactoring (`design.md`)**:
   - Complete 8-module architecture (① to ⑧) mapped to explicit event-driven workflows.
   - Concrete Tool Calling function signatures and JSON schemas.
   - Deterministic algorithm pseudocode (Algorithm 1) with line numbers.
4. **Academic Proposal Enhancements**:
   - Introduction refactor adhering to G~L structure supported by 2024–2026 empirical evidence.
   - Related Work refactor adhering to M~O structure with a 5-paper Article Summary Table and clear Research Gap synthesis.

## 3. Risks & Mitigations
* **Risk 1: LLM Numerical Inconsistency**
  * *Mitigation*: Numerical calculations are strictly quarantined to deterministic Python functions via whitelisted Function Calling.
* **Risk 2: Cultural Food Recognition Failure**
  * *Mitigation*: Multi-tier retrieval integrating ChromaDB (local snacks), Taiwan FDA Food Composition Database, and fallback ingredient decomposition.
* **Risk 3: Unethical Weight-Loss Coaching**
  * *Mitigation*: Hardcoded safety guardrail enforcing a minimum caloric floor ($< 1,200\text{ kcal}$) and eating-disorder keyword triggers.

