# Use Trunk-Based Development with Short-Lived PR Branches

* **ID:** 003
* **Date:** 2026-11-14
* **Status:** Accepted — records the policy adopted in [PR #1](https://github.com/land-foundry/github-governance/pull/1)
* **Authors:** Land Foundry Engineering

## Context

Land Foundry needs frequent integration of small, reviewable engineering changes while keeping the integration branch releasable. Human contributors and engineering agents share the same review and validation controls.

Long-lived development branches, including a separate integration branch as in Git Flow, defer integration and add reconciliation work. Direct commits to the integration branch simplify branching but do not provide the required PR review boundary. Short-lived PR branches support frequent integration through the existing review gates.

## Decision

Use trunk-based development with `main` as the authoritative integration branch and short-lived PR branches for changes requiring review. Discourage long-lived development branches.

Keep branching independent of environment deployment and release promotion. This decision records the existing model; it does not introduce a new workflow or change GitHub settings.

The [Source Control & Change History Governance](../source-control-change-history-governance.md#3-branching-strategy) owns branch lifecycle, naming, protection, and integration requirements. The [pull request workflow](../pull-request-workflow.md#short-lived-branches) describes the contributor procedure. [ADR-002](002-merge-commits.md) owns the merge-method decision.

## Consequences

* **Positive Impacts:** Frequent integration reduces divergence and makes changes available for review and validation in small increments. A single integration branch gives people and agents a consistent starting point.
* **Negative Impacts / Trade-offs:** Contributors must decompose larger initiatives and keep branches current. Work that is incomplete may need feature flags or equivalent isolation to preserve a releasable `main`.
* **Risks:** Branches can become long-lived when changes are too broad or review is delayed. Mitigate through small scopes, prompt review, and the policy's integration cadence and validation requirements.
