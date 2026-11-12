# OpenSpec Tasks: DietSense Proposal Refactoring & Implementation Plan

- [x] **Task 1: OpenSpec Scaffolding & Specification Definition**
  - [x] Create `openspec/config.yaml` specifying context, rules, and proposal standards.
  - [x] Author formal specification `specs/dietsense-core-system.md` with SHALL requirements and WHEN/THEN scenarios.
  - [x] Author clinical safety specification `specs/safety-and-guardrails.md`.

- [x] **Task 2: Academic Structural Synthesis (A to F, Cores, Trade-offs, Details)**
  - [x] Synthesize A to F narrative abstract in Traditional Chinese and IEEE Transactions journal format.
  - [x] Articulate the 5 domain mental models (Q1) with citations and system implications.
  - [x] Structure the 3 professional controversies (Q2) with opposing views, evidence, consensus, and open problems.
  - [x] Define 10 in-depth evaluative questions (Q3) covering Why, What-if, Trade-off, Failure Case, Evidence, Comparison, and Real-world Scenarios.

- [x] **Task 3: Proposal Document Refactoring**
  - [x] Refactor Introduction according to 2020 A-Z SOP (G~L) with 2024–2026 empirical evidence.
  - [x] Refactor Related Work according to M~O with 50% AI + 50% Domain balance.
  - [x] Construct 5-paper Article Summary Table and synthesize the Research Gap.

- [x] **Task 4: Workflow Conversion (`design.md`)**
  - [x] Convert conceptual 8 modules into an event-driven Finite State Machine (FSM).
  - [x] Define formal JSON schemas for whitelisted Function Calling tools.
  - [x] Document Algorithm 1 with explicit line numbers and error handling.
  - [x] Detail the end-to-end "beef noodle soup with egg" verification walkthrough.

- [ ] **Task 5: Dataset & Benchmark Preparation (Planned Implementation Phase)**
  - [ ] Curate 500 annotated Traditional Chinese meal descriptions spanning night market snacks, lunchboxes, noodle soups, and beverages.
  - [ ] Establish ground-truth nutritional intervals using Taiwan FDA Food Composition DB and USDA FoodData Central.

- [ ] **Task 6: Empirical User Study & Baseline Evaluation (Planned Implementation Phase)**
  - [ ] Recruit 20 participants for a 7-day longitudinal dietary logging study.
  - [ ] Compare DietSense with manual MyFitnessPal entry across logging time, SUS usability score, and ±20% caloric accuracy.
  - [ ] Verify safety guardrail compliance ($<1,200\text{ kcal}$ and eating-disorder phrase interceptions).

