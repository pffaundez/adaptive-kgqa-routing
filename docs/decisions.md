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


## D-007 — Controlled router instantiation

- **Date:** 2026-09-30
- **Status:** Active
- **Context:** The scientific contribution is pre-execution routing among KGQA workflows, not a new predictor architecture.
- **Decision:** Instantiate the primary router as a deliberately simple multi-task quality-cost predictor with a frozen query encoder, permitted pre-execution features, a shared MLP trunk, and workflow-specific quality and cost heads. Select workflows by maximizing predicted quality minus validation-normalized predicted cost. Treat direct classification and independent regressors as baselines.
- **Justification:** Separate quality and cost predictions support multiple deployment budgets without redefining workflow labels for each cost preference. A simple predictor reduces the risk that results are driven by unnecessary architectural complexity.
- **Alternatives considered:** Direct best-workflow classification as the primary router; sequential reinforcement learning; graph neural routing; end-to-end encoder fine-tuning.
- **Consequences:** The predictor architecture is not claimed as the main novelty. Router inputs, losses, cost normalization, and calibration must be frozen before test evaluation.
- **Affected files or experiments:** docs/experimental-design.md, E-001, Phase B, Phase C.

## D-008 — Bounded counterfactual-data acquisition

- **Date:** 2026-09-30
- **Status:** Active
- **Context:** Exhaustive execution of every workflow across full KGQA benchmarks may be prohibitively expensive.
- **Decision:** Begin with a stratified 600-query training pilot: 200 queries from each primary dataset and 2,400 workflow executions for four workflows. If the oracle-headroom gate passes, construct an initial dataset containing 3,000 training, 600 validation, and 900 test queries, corresponding to at most 18,000 workflow executions. Require a separately approved decision before further expansion.
- **Justification:** The bounded design is sufficient for a frozen-encoder router pilot, supports dataset-level evaluation, and establishes an explicit compute ceiling. Learning curves must justify additional acquisition.
- **Alternatives considered:** Full-benchmark exhaustive generation; an unbounded adaptive collection process; training only on the 600-query pilot.
- **Consequences:** Pilot outcomes may be reused only within the training population and only if workflows, prompts, model revision, KG snapshot, and instrumentation remain unchanged. Test counterfactual outcomes remain unavailable for router training and validation decisions.
- **Affected files or experiments:** docs/experimental-design.md, docs/next-steps.md, E-001, Phase A.

## D-009 — Controlled model assignment

- **Date:** 2026-09-30
- **Status:** Active
- **Context:** Workflow comparisons must not be confounded by assigning different language models to different workflows.
- **Decision:** Use the frozen `Qwen/Qwen2.5-14B-Instruct` backbone for W1–W4 and the frozen `intfloat/e5-base-v2` encoder for the router. The primary selection space contains workflows only, not model–workflow pairs. Run the backbone in BF16 without quantization and use deterministic greedy inference. Pin exact model and tokenizer revisions before Phase A.
- **Justification:** A shared, stable backbone isolates the effect of workflow structure while preserving sufficient capacity for KGQA generation.
- **Alternatives considered:** Qwen 3; smaller Qwen variants; heterogeneous workflow-specific models; joint model–workflow routing.
- **Consequences:** Model heterogeneity is excluded from the primary experiment and requires separate approval as a later extension.
- **Affected files or experiments:** docs/experimental-design.md, configs, Phase A–C.

## D-010 — Primary cost definition

- **Date:** 2026-09-30
- **Status:** Active
- **Context:** The router requires a reproducible cost target comparable across workflows with different numbers of calls.
- **Decision:** Define primary workflow cost as total LLM tokens summed across every call, including input and output tokens. Retain input tokens, output tokens, call count, latency, GPU time, KG operations, and repair attempts as disaggregated secondary measures. Measure router overhead separately and include it only in end-to-end reporting.
- **Justification:** Total token consumption is deterministic, auditable, and directly comparable across the controlled workflows.
- **Alternatives considered:** Latency only; GPU time only; number of calls; monetary API cost.
- **Consequences:** Every workflow call must emit complete usage metadata, including unsuccessful executions.
- **Affected files or experiments:** Phase A counterfactual records, Phase C evaluation.

## D-011 — Reproducibility and seed protocol

- **Date:** 2026-09-30
- **Status:** Active
- **Context:** Reviewers must be able to reproduce downstream analyses even when exact GPU-level regeneration is not bitwise identical.
- **Decision:** Use dataset sampling seed 42; router training seeds 0, 1, 2, 3, and 4; and 10,000 paired query-bootstrap samples with seed 42. Report every router run rather than selecting the best. Use greedy generation, deterministic retrieval tie ordering, one immutable canonical result per query–workflow pair, and explicit query-ID splits. Pin model, tokenizer, prompt, configuration, and software revisions. Preserve raw counterfactual outputs, checksums, and environment and hardware metadata.
- **Justification:** This protocol separates experimental variability from artifact identity and enables exact reproduction of downstream evaluation from immutable outputs.
- **Alternatives considered:** A single training seed; best-seed reporting; regeneration without preserved outputs.
- **Consequences:** The project does not claim bitwise-identical GPU regeneration; exact downstream reproduction relies on released immutable artifacts.
- **Affected files or experiments:** Phase A–C, artifact release.

## D-012 — Shared KG and retrieval infrastructure

- **Date:** 2026-09-30
- **Status:** Active with sensitivity provision
- **Context:** Infrastructure differences could be mistaken for workflow complementarity.
- **Decision:** Use the same checksummed Freebase database from `dki-lab/Freebase-Setup`, a local read-only Virtuoso instance, and a shared ontology. Use one frozen automatic entity linker based on GrailQA BERT-NER with FACC1/Freebase aliases, and provide the same ranked entity candidates to every workflow; do not use gold entities in the primary condition. Workflows may retrieve differently after this shared input, but every retrieval event must be logged. Before counterfactual generation, audit executable gold logical forms, gold-answer reachability, missing entities and relations, and answer agreement.
- **Justification:** Shared infrastructure controls avoid attributing database or entity-linking variation to routing quality.
- **Alternatives considered:** Workflow-specific entity linkers; gold entities; different KG snapshots.
- **Consequences:** If the audit reveals substantial or dataset-specific linker failures, run a separate sensitivity diagnostic with an alternative linker or gold-entity upper bound. This diagnostic cannot silently replace the primary condition; a permanent change requires approval.
- **Affected files or experiments:** Phase A0 audit, Phase A counterfactual generation.

## D-013 — Workflow execution contracts

- **Date:** 2026-09-30
- **Status:** Active with pilot sensitivity provision
- **Context:** The four workflows require common interfaces and bounded execution to support valid quality–cost comparisons.
- **Decision:** Every workflow must return a canonical S-expression that is deterministically translated to SPARQL and executed by the shared Freebase executor. Answers come only from execution, never free text. Record standardized statuses such as `success`, `parse_error`, `execution_error`, `timeout`, and `empty_result`; assign unsuccessful outcomes quality zero while retaining observed cost.

  Execution budgets are: W1 at most one LLM call; W2 at most four calls, comprising up to three graph transitions and one logical-form synthesis; W3 at most one call; and W4 at most three calls, comprising one initial attempt and at most two execution-informed repairs. Each call has an 8,192-token context limit and a 512-token generation limit. Use `do_sample=false` and stop early on valid complete structured output. W4 may use execution feedback but never gold information.

  Retrieval budgets are: top five entity candidates per mention; maximum graph or schema depth three; W2 top 25 relations per step with beam width five; W3 and initial W4 up to 50 relation candidates and 20 type candidates; and each W4 repair up to 25 additional relations. Use frozen deterministic E5 candidate ranking and prohibit gold logical forms, relations, and answers.
- **Justification:** Explicit contracts make workflow capability differences measurable while bounding cost and preventing access to privileged supervision.
- **Alternatives considered:** Unbounded agent loops; workflow-specific output formats; free-text answers; gold-assisted retrieval.
- **Consequences:** A smoke test may identify overflow, empty candidates, latency, or implementation defects, but cannot tune quality. During the 600-query A0 pilot, inspect whether W2–W4 costs force selection toward W1 using a quality-only oracle, cost distributions, Pareto contribution, oracle-over-budget analysis, and exclusive wins. If W1 truly dominates on quality and cost, report that result. Budget changes require documented approval before full Phase A and cannot be made after test inspection.
- **Affected files or experiments:** Workflow implementations, Phase A0, Phase A–C.

## D-014 — Cost normalization and trade-off evaluation

- **Date:** 2026-09-30
- **Status:** Active with post-A0 sensitivity provision
- **Context:** Raw token counts require a stable reference scale for training and interpreting the quality–cost trade-off.
- **Decision:** Normalize workflow cost as raw total tokens divided by the mean W1 token cost on the training partition, without logarithmic transformation. Define utility as `Q_hat - lambda * C_tilde`. Evaluate lambda values `[0, 0.01, 0.02, 0.05, 0.10, 0.20, 0.50, 1, 2]` and normalized target budgets `[1, 1.25, 1.5, 2, 3]`. Select lambda on validation data only and freeze it before test evaluation. Report the complete quality–cost frontier, Pareto-efficient points, matched-cost and matched-quality comparisons, area under the common frontier interval, and workflow selection frequencies.
- **Justification:** W1-based linear normalization preserves relative cost and makes operating points interpretable without hiding expensive workflows.
- **Alternatives considered:** Raw cost only; log-normalized cost; a single fixed lambda; test-set tuning.
- **Consequences:** After the 600-query A0 pilot and before full counterfactual generation, the lambda range or density and matched-budget points may be adjusted only if frontier coverage is insufficient. Any adjustment must use A0 evidence, preserve original A0 results, be documented and approved, and remain frozen for test evaluation.
- **Affected files or experiments:** Router objective, Phase A0, Phase B–C.

