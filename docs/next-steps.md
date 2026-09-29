# Next Steps

## Now

### T-001 — Define and approve the minimal pilot workflow portfolio

- **Objective:** Select the smallest workflow set capable of testing whether query-dependent routing has meaningful quality-cost headroom.
- **Dependencies:** D-001, D-002, D-003, and D-004.
- **Completion criterion:** The user approves a versioned workflow list with operational definitions, shared inputs, output schema, stopping conditions, model assignments, and expected cost differences. The experimental decision is recorded before implementation.
- **Restrictions:** Do not implement workflows, generate outcomes, or expand the portfolio before approval.
- **Status:** Active.
- **Priority:** Critical.
- **Blocker:** The workflow portfolio is not yet defined.

## Next

### T-002 — Freeze the pilot population and split policy

- **Objective:** Define a stratified query sample for the oracle-headroom pilot.
- **Dependencies:** Completion of T-001.
- **Completion criterion:** Approved datasets, source versions, query strata, sample counts, split source, deduplication policy, and dataset fingerprints.
- **Restrictions:** Do not sample from test for training or validation. Preserve dataset-specific leakage controls.
- **Status:** Pending.
- **Priority:** Critical.
- **Blocker:** Requires the workflow portfolio.

### T-003 — Specify the counterfactual outcome schema

- **Objective:** Turn the fields in experimental-design.md into a validated, versioned data contract.
- **Dependencies:** Completion of T-001 and T-002.
- **Completion criterion:** Schema, validation rules, identifiers, cost units, version fields, and synthetic integrity tests are approved.
- **Restrictions:** Do not implement a runner against an unstable schema.
- **Status:** Pending.
- **Priority:** High.
- **Blocker:** Requires frozen workflows and pilot population.

## Later

### T-004 — Implement workflow adapters and the counterfactual runner

- **Status:** Deferred.
- **Activation condition:** T-001 through T-003 completed.

### T-005 — Implement oracle and metric validation

- **Status:** Deferred.
- **Activation condition:** Counterfactual outcome schema approved.

### T-006 — Implement and compare routing baselines

- **Status:** Deferred.
- **Activation condition:** Pilot outcomes pass integrity checks.

### T-007 — Evaluate external cross-KG validation

- **Status:** Deferred.
- **Activation condition:** Controlled Freebase experiment justifies further investment and the user approves expansion.

### T-008 — Prepare the public paper artifact

- **Status:** Deferred.
- **Activation condition:** Paper artifact preparation begins.
- **Required gate:** Remove or exclude internal continuity documents after preserving necessary scientific information in public-facing documentation.

## Blocked

No separate blocked tasks. The active blocker is recorded under T-001.

## Completed

### T-000 — Bootstrap and validate the repository environment

- **Result:** Repository created, published, cloned on kiowl, installed in an isolated Python 3.11 environment, and validated with pytest and Ruff.
- **Evidence:** Commit 03c3c00aff9195876ad1df29c11396ecf9aab1a7; one test passed; Ruff reported no errors on 2026-09-29.
- **Status:** Completed.
