# Next Steps

## Now

### T-004 — Prepare and execute the Phase A0 pilot

- **Objective:** Implement the minimum reproducible pipeline required to determine whether W1–W4 exhibit meaningful query-dependent complementarity.
- **Dependencies:** D-001 through D-015.
- **Completion criterion:** The shared Freebase infrastructure passes its compatibility audit; the workflow outcome schema and W1–W4 adapters pass integrity tests; and all 600 approved pilot queries are executed under the frozen contracts.
- **Required outputs:** The workflow performance matrix, raw immutable outputs, checksums, cost records, standardized failure statuses, and a reproducible A0 analysis report.
- **Go/no-go evidence:** Workflow disagreement, quality-oracle headroom, utility-oracle headroom, exclusive wins, Pareto contribution, query-type coverage, and outcome-collection cost.
- **Restrictions:** Do not expand the dataset, alter workflow budgets, or train the primary router before the A0 gate is reviewed and approved.
- **Status:** Active.
- **Priority:** Critical.
- **Blocker:** None at the design level; implementation and infrastructure validation are now required.

## Next

### T-005 — Review the A0 gate

- **Objective:** Decide whether the controlled workflow portfolio justifies full benchmark construction.
- **Dependencies:** Completion of T-004.
- **Completion criterion:** A documented user-approved go, revise, or stop decision based only on the frozen A0 diagnostics.
- **Restrictions:** Preserve the original A0 results. Any approved revision must be recorded before new outcome collection.
- **Status:** Pending.
- **Priority:** Critical.

### T-006 — Construct the full workflow selection benchmark

- **Objective:** Populate the approved 3,000/600/900 train/validation/test workflow performance matrices.
- **Dependencies:** A positive T-005 decision.
- **Completion criterion:** At most 18,000 validated query–workflow records under one frozen protocol.
- **Status:** Pending.
- **Priority:** High.

### T-007 — Train and evaluate routing policies

- **Objective:** Execute Phases B and C using the frozen benchmark, baselines, router, metrics, and statistical protocol.
- **Dependencies:** Completion of T-006.
- **Completion criterion:** Reproducible quality–cost frontiers, oracle comparisons, ablations, and uncertainty estimates.
- **Status:** Pending.
- **Priority:** High.

## Later

### T-008 — Evaluate external cross-KG validation

- **Status:** Deferred.
- **Activation condition:** The controlled Freebase experiment produces sufficient positive evidence and the user approves expansion.

### T-009 — Prepare the public paper artifact

- **Status:** Deferred.
- **Activation condition:** Experiments are complete and results support the paper.
- **Required gate:** Remove or exclude internal continuity documents after preserving necessary scientific information in public-facing documentation.

## Completed

### T-000 — Bootstrap and validate the repository environment

- **Result:** Repository created, published, cloned on kiowl, installed in an isolated Python 3.11 environment, and validated with pytest and Ruff.
- **Evidence:** Commit 03c3c00aff9195876ad1df29c11396ecf9aab1a7; one test passed; Ruff reported no errors on 2026-09-29.
- **Status:** Completed.

### T-001 — Define and approve the minimal pilot workflow portfolio

- **Result:** W1–W4, shared inputs, output contract, execution budgets, retrieval limits, model assignment, and cost accounting were approved.
- **Evidence:** D-005, D-006, D-009, D-010, D-012, and D-013.
- **Status:** Completed.

### T-002 — Freeze the pilot population and split policy

- **Result:** A 600-query training pilot was approved, with 200 queries from each primary dataset and dataset-specific leakage controls.
- **Evidence:** D-003, D-008, and D-011.
- **Status:** Completed.

### T-003 — Specify the workflow outcome record requirements

- **Result:** Required identifiers, outcomes, costs, versions, statuses, reproducibility metadata, and information boundaries were defined at the protocol level. The implementation schema and validators remain part of T-004.
- **Evidence:** D-002, D-010, D-011, and D-013.
- **Status:** Completed.
