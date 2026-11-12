# Use Merge Commits with Curated Engineering History

* **ID:** 002
* **Date:** 2026-11-12
* **Status:** Accepted — records the policy adopted in [PR #1](https://github.com/land-foundry/github-governance/pull/1)
* **Authors:** Land Foundry Engineering
* **Supersedes:** [ADR-001: Use squash merges for pull requests](001-squash-merges.md)

## Context

Land Foundry treats source control history as an engineering knowledge asset for people and agents. PRs explain cohesive engineering outcomes and rationale; commits explain intentional implementation changes. Squash merging collapses those implementation boundaries, even when the commits provide useful context.

## Decision

Use merge commits as the standard PR integration method, preserving curated Conventional Commits and an explicit PR integration boundary. Consolidate development checkpoints before integration unless they represent independently meaningful engineering changes.

Apply this through trunk-based development with short-lived PR branches, contextual PR descriptions, and the existing review and validation gates. Follow the [Source Control & Change History Governance](../source-control-change-history-governance.md) and [pull request workflow](../pull-request-workflow.md); alternative merge methods require a documented, approved exception.

## Consequences

* **Positive Impacts:** Preserves progressive disclosure from PR outcomes to implementation details, original commit identities, and the integration boundary.
* **Negative Impacts / Trade-offs:** History is more complex than a linear squash history, and authors and reviewers must curate meaningful commit boundaries.
* **Risks:** Merge commits can preserve low-value checkpoints. Mitigate through history review and curation before merging. Repository settings must permit merge commits and must not require linear history; squash and rebase merging should be disabled unless an approved exception applies. This ADR does not change or verify those settings.
