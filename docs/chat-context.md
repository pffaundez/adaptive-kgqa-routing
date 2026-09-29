# Chat Context

## Purpose

This repository studies pre-execution, query-dependent routing among heterogeneous Knowledge Graph Question Answering (KGQA) workflows.

The central question is whether a router can predict which workflow offers the best answer-quality and execution-cost trade-off for each query without observing candidate outputs.

## Current Method

For a query q and workflow portfolio W, the router predicts workflow-specific answer quality and cost and selects the workflow maximizing predicted quality minus a cost penalty.

Training and validation use counterfactual outcomes obtained by running every workflow on every query. At test time, the router may use only pre-execution information and executes only the selected workflow.

## Experimental Scope

The controlled benchmark suite is:

- WebQuestionsSP;
- ComplexWebQuestions, with seed-safe partitions;
- GrailQA, reported separately for i.i.d., compositional, and zero-shot regimes.

KQA Pro is deferred to external cross-KG validation. QALD is not part of the primary counterfactual-training suite.

The scientific contract is in experimental-design.md.

## Verified Repository State

- Canonical branch: main.
- Initial repository commit: 03c3c00aff9195876ad1df29c11396ecf9aab1a7.
- Python requirement: 3.11 or newer.
- Build backend: Hatchling.
- Package layout: src.
- Runtime dependencies: none currently declared.
- Development dependencies: pytest and Ruff.
- Baseline validation on 2026-09-29: one test passed and Ruff reported no errors.
- No workflow, router, dataset loader, experiment runner, or evaluator has been implemented.
- No scientific experiment has been executed or validated.

## Working Rules

- Keep repository content, code comments, configurations, and documentation in English.
- Maintain exactly one active task.
- Prefer convergence and falsifiable gates over opening parallel experimental branches.
- Methodological ideas and expansions require user approval.
- Do not present design, partial implementation, or unvalidated output as a result.
- Preserve unknown working-tree changes and inspect them before editing.
- Remove or exclude internal continuity documents before publishing a paper artifact, after transferring necessary scientific information to public-facing documentation.

## Scientific Constraints

- Candidate outputs, paths, critiques, and realized execution lengths are prohibited router inputs in the pre-execution condition.
- Do not merge WebQuestionsSP and ComplexWebQuestions through a naive random split.
- Do not run a full sweep before the stratified pilot and oracle-headroom gate.

## Active Task

T-001: define and obtain approval for the minimal pilot workflow portfolio.

Continue from next-steps.md, consult decisions.md for experimental decisions, and update experiment-status.md only with verified facts.
