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


## D-005 — Controlled modular workflow portfolio

- **Date:** 2026-09-29
- **Status:** Active
- **Context:** The routing experiment requires workflows with meaningfully different reasoning capabilities while avoiding unnecessary implementation confounds.
- **Decision:** Use a controlled modular portfolio comprising direct specialized execution, graph-constrained reasoning, compositional logical-form execution, and bounded adaptive verification and repair. Share the KG snapshot, entity-linking inputs, schema information, answer format, execution engine, and cost accounting whenever technically possible. Treat published end-to-end KGQA systems as external baselines rather than portfolio members.
- **Justification:** The four workflows isolate increasing reasoning and verification capabilities while preserving a common experimental environment.
- **Alternatives considered:** Mixing complete published systems as portfolio members; collapsing logical-form execution into graph-constrained reasoning; using only three workflows.
- **Consequences:** Concrete model assignments and implementations must be controlled separately before the portfolio is considered frozen. Differences attributed to workflows must not be confounded with unrelated model or infrastructure differences.
- **Affected files or experiments:** docs/experimental-design.md, docs/next-steps.md, E-001.


## D-006 — Staged control of model heterogeneity

- **Date:** 2026-09-30
- **Status:** Active
- **Context:** Assigning substantially different models to different workflows would confound workflow selection with model selection.
- **Decision:** Use one shared frozen backbone across all technically compatible workflows in the primary oracle-headroom pilot. Introduce model heterogeneity only in a subsequent approved stage, using a crossed model-workflow design when feasible, and only if the controlled pilot demonstrates sufficient workflow-dependent headroom.
- **Justification:** Holding the backbone fixed isolates the effect of the reasoning workflow before studying model-workflow interactions.
- **Alternatives considered:** Assigning a different model to each workflow from the beginning; treating model-workflow packages as indivisible actions.
- **Consequences:** W1 is defined as direct execution with the shared backbone, not as a small-model workflow. Claims about workflow effects must come from model-controlled comparisons. A later heterogeneous stage must report model effects and workflow-model interactions separately.
- **Affected files or experiments:** docs/experimental-design.md, docs/next-steps.md, E-001.
