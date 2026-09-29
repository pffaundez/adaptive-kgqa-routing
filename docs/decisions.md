# Experimental Decision Log

This file records only scientific and experimental decisions. Repository workflow, chat coordination, and internal document lifecycle rules do not belong in this log.

## D-001 — Pre-execution KGQA workflow routing

- **Date:** 2026-09-29
- **Status:** Active
- **Context:** The project requires a query-dependent decision over heterogeneous KGQA workflows while controlling execution cost.
- **Decision:** Evaluate a router that selects one workflow before candidate execution by predicting workflow-specific answer quality and cost.
- **Justification:** This isolates the value of pre-execution routing and avoids paying for all candidate outputs at deployment time.
- **Alternatives considered:** Fixed workflow selection; post-execution answer selection; executing the full workflow portfolio.
- **Consequences:** Router inputs must be restricted to information available before workflow execution.
- **Affected files or experiments:** README.md, docs/experimental-design.md, E-001.

## D-002 — Counterfactual supervision and information boundary

- **Date:** 2026-09-29
- **Status:** Active
- **Context:** Workflow-specific quality labels are not available before execution.
- **Decision:** Construct training and validation supervision by executing every candidate workflow per query. Prohibit candidate answers, generated paths, critiques, and realized execution lengths as router inputs in the pre-execution condition.
- **Justification:** Exhaustive outcomes provide supervised targets while the information boundary preserves the claimed deployment setting.
- **Alternatives considered:** Online reinforcement learning; post-execution selection; heuristic-only routing.
- **Consequences:** Counterfactual generation is an offline experimental cost. Test outcomes may be used retrospectively for oracle analysis but not router selection or tuning.
- **Affected files or experiments:** docs/experimental-design.md, E-001.

## D-003 — Controlled Freebase benchmark suite

- **Date:** 2026-09-29
- **Status:** Active
- **Context:** The main experiment requires scale, structural diversity, and a shared execution environment.
- **Decision:** Use WebQuestionsSP, ComplexWebQuestions, and GrailQA as the controlled primary suite. Keep their results separate and use seed-safe handling for ComplexWebQuestions. Defer KQA Pro to external cross-KG validation.
- **Justification:** The suite supports lower-complexity, compositional, and generalization analyses while reducing infrastructure confounds.
- **Alternatives considered:** QALD as the primary source; immediately mixing Freebase and Wikidata benchmarks.
- **Consequences:** Initial claims are limited to English-language Freebase KGQA. Multilingual and cross-KG generalization remain out of scope for the primary experiment.
- **Affected files or experiments:** README.md, docs/experimental-design.md, E-001.

## D-004 — Pilot gate before full counterfactual generation

- **Date:** 2026-09-29
- **Status:** Active
- **Context:** Exhaustive workflow execution may be expensive, and routing is unjustified if workflows do not exhibit query-dependent complementarity.
- **Decision:** Run a stratified pilot before generating the full counterfactual dataset. The pilot must measure workflow disagreement, quality-oracle headroom, utility-oracle headroom, query-type coverage, and generation cost.
- **Justification:** This provides an explicit early stopping condition and prevents an unjustified full sweep.
- **Alternatives considered:** Immediate full counterfactual generation.
- **Consequences:** Workflow portfolio and pilot population must be frozen before execution.
- **Affected files or experiments:** docs/experimental-design.md, E-001.
