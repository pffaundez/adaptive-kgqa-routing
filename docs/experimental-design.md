# Experimental Design

## Objective

Evaluate whether a pre-execution router can improve the quality-cost frontier over the strongest deployable fixed or learned policy when selecting among heterogeneous KGQA workflows.

## Counterfactual outcome dataset

For each query \(q\) and workflow \(w\), record:

- dataset, query ID, split, question, and gold answers;
- gold logical form and query type when available;
- predicted answers and observed task quality;
- execution success and diagnostic error type;
- LLM calls, input/output tokens, latency, KG operations, and monetary cost;
- workflow version, model version, prompt version, seed, and KG snapshot.

Router inputs must contain only signals available before candidate execution. Candidate answers, generated paths, critiques, and realized execution lengths are prohibited inputs for the pre-execution condition.

## Datasets

### Controlled Freebase suite

1. **WebQuestionsSP** provides a lower-complexity anchor.
2. **ComplexWebQuestions** provides compositional operations; use a partition that groups questions derived from the same seed.
3. **GrailQA** provides i.i.d., compositional, and zero-shot evaluation.

Keep datasets separate in reporting. Do not randomly merge WebQuestionsSP and ComplexWebQuestions because of their derivational relationship.

### External validation

Use **KQA Pro** only after the core study to evaluate richer operators and transfer to another KG representation. Treat the required workflow adaptations as an experimental factor rather than assuming direct comparability.

## Data partitions

### Training

- execute every workflow for every training query;
- compute observed quality against gold answers;
- log every cost dimension;
- train quality and cost prediction using pre-execution features only.

### Validation

Use validation to select hyperparameters, normalize cost, select \(\lambda\), define budgets and equivalence tolerance, tune heuristic thresholds, and choose stopping checkpoints.

### Test

1. Extract permitted pre-execution features.
2. Predict quality and cost for each workflow.
3. Select and execute one workflow.
4. Measure realized quality and cost.
5. Use exhaustive test outcomes only afterward to compute oracles and regret.

## Baselines

### Fixed policies

- cheapest workflow;
- best-average workflow selected on validation;
- strongest workflow;
- full pipeline, when defined.

### Non-learned policies

- random routing over multiple seeds;
- query-complexity rules;
- entity-linking-confidence rules;
- reachability-only routing;
- a combined rule-based policy.

### Learned policies

- direct multiclass workflow classifier;
- independent quality predictor;
- quality-and-cost predictor;
- pairwise workflow ranker.

### Ablations

- without cost modelling;
- without structural query features;
- without KG-derived features;
- without expressive-adequacy or semantic-risk auxiliary supervision;
- independent scores versus the proposed joint formulation.

All deployable baselines must receive equivalent pre-execution information.

## Oracles

For observed quality \(Q(q,w)\) and normalized cost \(\widetilde C(q,w)\):

### Quality oracle

\[
w_Q^*(q)=\arg\max_w Q(q,w).
\]

### Utility oracle

\[
w_U^*(q;\lambda)=\arg\max_w
\left[Q(q,w)-\lambda\widetilde C(q,w)\right].
\]

### Budget-constrained oracle

\[
w_B^*(q)=\arg\max_{w:C(q,w)\leq B} Q(q,w).
\]

When workflows are equivalent within a validation-defined tolerance \(\epsilon\), prefer the least expensive one. Report oracle headroom relative to the best fixed workflow before interpreting routing improvements.

## Metrics

### End-to-end KGQA quality

- the official primary metric for each benchmark;
- Answer Exact Match and Answer F1 where applicable;
- Hits@1, accuracy, logical-form match, or execution accuracy when appropriate;
- disaggregated results by reasoning type and generalization regime.

### Cost

- LLM calls;
- input and output tokens;
- monetary cost using a versioned price table;
- end-to-end and routing latency;
- KG queries, expansions, or inspected triples;
- material local compute and memory differences.

### Routing

- realized quality and cost;
- utility across predefined \(\lambda\) values or budgets;
- regret relative to the utility oracle;
- oracle recovery relative to the best fixed policy;
- cost-quality Pareto frontier;
- workflow-selection accuracy as a diagnostic only.

### Prediction

- MAE or RMSE for quality and cost;
- Spearman or Kendall rank correlation across workflows;
- Brier score, ECE, and reliability diagrams for probabilistic predictions.

## Statistics

- paired bootstrap confidence intervals over queries;
- paired tests on identical query sets;
- multiple-comparison correction where applicable;
- effect sizes in addition to p-values;
- mean, dispersion, and per-seed results for stochastic workflows.

## Pilot go/no-go gate

Before generating the full counterfactual dataset, run a stratified pilot and measure:

1. workflow disagreement;
2. quality-oracle headroom;
3. utility-oracle headroom;
4. representation of all major query types;
5. counterfactual-generation cost.

Proceed only if the workflow portfolio exhibits meaningful query-dependent complementarity. The final method must improve the test cost-quality frontier over the strongest deployable baseline; approaching an oracle alone is insufficient.

