# Experiment Status

## Status Vocabulary

- **Designed:** the scientific protocol is specified.
- **Implemented:** executable code exists and passes its implementation checks.
- **Executed:** the experiment completed and produced structurally valid outputs.
- **Validated:** outputs passed integrity checks and metrics were independently verified.
- **Interpreted:** conclusions were evaluated against baselines, uncertainty, costs, and threats to validity.

These states are not interchangeable.

## Designed

### E-001 — Pre-execution KGQA workflow routing

**State:** Partially designed.

The repository defines:

- the pre-execution information boundary;
- counterfactual outcome collection;
- train, validation, and test roles;
- baseline families;
- quality, utility, and budget-constrained oracles;
- quality, cost, routing, prediction, and statistical evaluation dimensions;
- a stratified pilot and oracle-headroom go/no-go gate.

The following material definitions remain open:

- minimal workflow portfolio;
- exact KG snapshot;
- pilot query population and sample size;
- operational workflow interface;
- normalized cost definition and candidate lambda values;
- primary metric per benchmark;
- stochastic repetition policy and final seed set.

Because these definitions can change the meaning of the result, no pilot may start before they are approved and recorded.

## Implemented

- Python package scaffold.
- Hatchling packaging configuration.
- Example experiment configuration.
- One package-version smoke test.
- Repository-level Ruff configuration.

No dataset loader, workflow implementation, router, counterfactual runner, oracle calculator, metric implementation, or result validator exists.

## Executed

No scientific experiments have been executed.

Environment checks executed on kiowl on 2026-09-29:

- Python 3.11.2;
- editable installation of package version 0.1.0;
- pytest 9.1.1;
- Ruff 0.16.9;
- one smoke test passed;
- Ruff reported no errors.

These are engineering validations, not scientific results.

## Validated

No experimental output or scientific metric has been validated.

## Results and Artifacts

No counterfactual dataset, model checkpoint, experiment output, aggregate result, or frozen artifact exists.

## Claims Currently Permitted

- The repository contains a high-level protocol for pre-execution, cost-aware KGQA workflow routing.
- The initial Python scaffold installs and passes its current smoke test and lint check on the verified kiowl environment.

## Claims Not Yet Supported

- Routing improves answer quality, cost, utility, or the Pareto frontier.
- The workflow portfolio exhibits query-dependent complementarity.
- Any learned router approaches an oracle.
- Results generalize across datasets, schemas, KGs, models, or languages.
- Expressive-adequacy or semantic-risk supervision improves routing.

## Known Limitations

The current repository is a research scaffold. Its scientific design is not yet executable because the workflow portfolio and pilot population are not frozen.
