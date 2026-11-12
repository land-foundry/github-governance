# Use Squash Merges for Pull Requests

* **ID:** 001
* **Date:** 2026-09-12
* **Status:** Superseded by [ADR-002: Use merge commits with curated engineering history](002-merge-commits.md)
* **Authors:** Matt Land

## Supersession

[ADR-002](002-merge-commits.md) records the replacement merge-strategy decision.

The replacement policy adopts merge commits with curated Conventional Commits and context-rich PRs, preserving progressive disclosure from engineering outcomes to implementation details. This supersession took effect when the replacement policy was merged in [PR #1](https://github.com/land-foundry/github-governance/pull/1).

The original decision and consequences below are retained as historical context. They no longer prescribe the workflow after adoption. See the [pull request workflow](../pull-request-workflow.md) for the replacement procedure. Adoption requires reviewing merge-method settings and removing any linear-history requirement that would prevent merge commits; this document does not change GitHub settings.

## Context

Repositories in the Land Foundry GitHub organization serve as a professional portfolio as well as an environment for experimenting with software engineering, architecture, automation, and cloud technologies.

A merge strategy is needed for integrating pull requests into protected branches such as `main`. The strategy should produce a repository history that is easy to understand while preserving enough information to review how changes were developed.

The following GitHub merge strategies were considered:

* **Merge commits** preserve all commits from the source branch and create an explicit merge commit when the pull request is integrated. This preserves branch topology but can make the permanent history more complex and may retain low-value development commits.
* **Rebase and merge** preserves individual commits while replaying them onto the target branch. This creates linear history but requires individual commits to be consistently meaningful and rewrites their commit identities.
* **Squash and merge** combines all commits from a pull request into a single commit on the target branch. This produces a concise linear history where each commit represents one logical change.

Although merge commits provide the greatest visibility into branch topology, GitHub pull requests already preserve important development context including individual commits, code review discussion, changed files, status checks, and review history.

The permanent history of `main` therefore does not need to duplicate the complete development history of each feature branch.

For these repositories, the pull request is considered the primary unit of change. The `main` branch should communicate the sequence of meaningful changes made to the repository rather than every intermediate development step.

## Decision

We will use **squash merging as the pull request merge strategy**.

Changes to protected branches such as `main` will be introduced through pull requests and merged using GitHub's **Squash and merge** option.

Each merged pull request will produce one commit on `main`.

For example, a feature branch may contain several development commits:

```text
          D---E---F---G
         /
A---B---C
```

After the pull request is squash merged, the permanent history becomes:

```text
A---B---C---S
```

Where `S` represents the combined changes from commits `D`, `E`, `F`, and `G`.

The original development history remains available through the pull request.

Because squash merging produces a linear target-branch history, repositories may enable the GitHub **Require linear history** branch rule.

Merge commits and rebase-and-merge will not be used for normal pull request integration.

Squash commit messages should describe the logical change introduced by the pull request rather than individual implementation steps.

## Consequences

Using squash merges creates a concise repository history where each commit on `main` represents a reviewed and accepted pull request.

Contributors may commit freely while developing a feature without requiring every intermediate commit to meet permanent-history standards.

The detailed development process remains available through GitHub pull requests, while the `main` branch provides a higher-level history of meaningful repository changes.

* **Positive Impacts:**

  * Produces a clean and linear `main` branch history.
  * Creates a direct relationship between one pull request and one commit.
  * Makes repository history easier to scan and understand.
  * Reduces permanent commit noise from temporary fixes, lint changes, debugging, or review adjustments.
  * Removes the need to routinely rewrite feature-branch history before merge.
  * Allows contributors to commit incrementally while developing a change.
  * Preserves detailed development and review context within the pull request.
  * Supports enabling GitHub's `Require linear history` rule.

* **Negative Impacts / Trade-offs:**

  * Individual feature-branch commits are not preserved as separate commits on `main`.
  * Branch topology and explicit merge points are not retained in the permanent Git history.
  * Repository history provides less information about the internal development sequence of a change without consulting the associated pull request.
  * Carefully constructed feature-branch commits are collapsed even when they could be individually useful.

* **Risks:**

  * **Squash commit messages may become vague or inconsistent.** Mitigate this by requiring pull request titles and squash commit messages to clearly describe the logical change.
  * **Important implementation history may be less visible from Git alone.** Mitigate this by preserving pull requests and using them as the source of detailed development and review context.
  * **Large pull requests may produce overly broad commits.** Mitigate this by encouraging small, focused pull requests that represent a single logical change.
