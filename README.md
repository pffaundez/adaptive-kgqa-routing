# Adaptive KGQA Routing

Research repository for pre-execution, query-dependent routing among heterogeneous Knowledge Graph Question Answering (KGQA) workflows.

## Research question

Can a router predict, before executing the candidates, which KGQA workflow offers the best quality-cost trade-off for each query?

For a query \(q\) and a workflow portfolio \(\mathcal W\), the initial decision rule is:

\[
\hat w(q;\lambda)=\arg\max_{w\in\mathcal W}
\left[\widehat Q(q,w)-\lambda\widehat C(q,w)\right],
\]

where \(\widehat Q\) and \(\widehat C\) are pre-execution predictions of answer quality and execution cost. Expressive adequacy and semantic-failure risk may later be evaluated as auxiliary signals, but are not assumed to be required components.

## Experimental principle

Training and validation use a counterfactual outcome table produced by executing every candidate workflow on every query. At test time, the router observes only pre-execution features, selects one workflow, and executes only that workflow. The full test outcome table is used afterward solely to calculate oracles and regret.

## Initial benchmark scope

- **WebQuestionsSP:** simpler natural-language Freebase questions and a test of whether expensive workflows can be avoided.
- **ComplexWebQuestions:** compositional questions, using a seed-safe revised split.
- **GrailQA:** i.i.d., compositional, and zero-shot schema generalization.
- **KQA Pro:** optional cross-KG and expressive-adequacy validation after the controlled Freebase experiment.

QALD is reserved for later multilingual or cross-KG external validation. It is not the primary source for counterfactual training because the current study prioritizes scale, structural diversity, and a shared execution environment.

## Candidate baseline families

- fixed policies: cheapest, best-average, strongest, and full workflow;
- non-learned policies: random, query-complexity, entity-linking-confidence, and reachability routing;
- learned policies: direct workflow classification, independent quality prediction, cost-aware quality prediction, and pairwise ranking;
- reproducible KGQA methods representing efficient collaboration, graph-constrained reasoning, adaptive planning, and semantic parsing;
- ablations of cost, query features, KG features, and auxiliary signals.

## Oracles

- **Quality oracle:** best observed workflow per query, ignoring cost.
- **Utility oracle:** best observed workflow according to quality minus normalized cost.
- **Budget-constrained oracle:** best observed workflow satisfying a fixed budget.

Oracles are retrospective analysis tools, not deployable baselines.

## Primary evaluation dimensions

- answer quality: dataset-official metric, Answer F1, Exact Match, Hits@1, or execution accuracy as applicable;
- execution cost: LLM calls, tokens, monetary cost, latency, and KG operations;
- routing: end-to-end utility, oracle regret, oracle recovery, and the cost-quality frontier;
- prediction: quality/cost error, ranking correlation, and calibration.

See [docs/experimental-design.md](docs/experimental-design.md) for the working protocol.

## Repository layout

```text
adaptive-kgqa-routing/
├── configs/       # Versioned experiment configurations
├── data/          # Local datasets and generated outcomes (not committed)
├── docs/          # Research protocol and design decisions
├── scripts/       # Reproducible data and experiment entry points
├── src/           # Python package
└── tests/         # Unit and integration tests
```

## Status

Early research scaffold. The workflow portfolio, KG snapshot, and final router architecture remain open experimental decisions.

