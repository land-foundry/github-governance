# Contributing

Thank you for contributing to this repository.

The goal of this project is to maintain a professional and understandable development history while using pull requests, automated checks, and branch protections to govern changes to `main`.

## Main Branch Merge Strategy

Changes to `main` must be introduced through a pull request.

This repository uses **Squash and merge** as the pull request merge strategy.

Each pull request represents one logical change and becomes one commit on `main`.

A feature branch may contain multiple development commits:

```text
          D---E---F---G
         /
A---B---C
```

When the pull request is squash merged, the resulting `main` history becomes:

```text
A---B---C---S
```

Where `S` represents the complete change introduced by the pull request.

The detailed feature-branch history, review discussion, changed files, and status checks remain available through the GitHub pull request.

## Pull Requests as the Unit of Change

This repository treats the **pull request as the primary unit of change**.

Pull requests should therefore be:

* Focused on a single logical change.
* Small enough to review effectively.
* Clearly titled.
* Accompanied by a description that explains the purpose of the change.
* Updated as necessary to reflect meaningful changes made during review.

The resulting squash commit should communicate what changed at a useful repository-history level.

For example:

```text
Add plugin-based rule discovery (#42)
```

is preferable to development-oriented messages such as:

```text
fix tests
lint
address review comments
```

## Feature Branch Commits

Contributors may make incremental commits while developing a feature.

Commits such as the following are acceptable during development:

```text
Implement registry
Add initial tests
Fix typing issue
Address review feedback
```

These commits do not need to be manually squashed or rebased before merging because GitHub will squash the pull request into a single commit.

Contributors should still avoid committing temporary debugging artifacts, secrets, generated files that do not belong in the repository, or unrelated changes.

Interactive rebasing may be used when useful, but it is not required as part of the normal contribution workflow.

## Pull Request Expectations

Before a pull request is merged into `main`:

* Required status checks must pass.
* Required reviews must be completed.
* Review conversations must be resolved.
* The pull request should represent a focused logical change.
* The pull request title should clearly describe the change.
* The pull request description should explain the purpose and relevant implementation details.
* Temporary debugging or work-in-progress changes must be removed.
* Unrelated changes should be moved to separate pull requests.

The pull request should then be merged using **Squash and merge**.

## Main Branch History

The `main` branch should provide a concise history of meaningful repository changes.

A healthy history should resemble:

```text
A  Initialize repository
B  Add contribution guidelines (#3)
C  Add repository rulesets (#7)
D  Add CI validation (#12)
E  Add plugin architecture (#18)
F  Add dependency resolution (#24)
```

Each commit should correspond to a reviewed pull request and represent one logical change.

This repository may use the GitHub **Require linear history** rule because squash merging does not create merge commits.

## Rationale

These repositories are intended to demonstrate professional software engineering practices.

A clean `main` history makes the evolution of the repository easy to understand, while GitHub pull requests preserve the more detailed development and review history.

This establishes a clear separation of responsibilities:

* **Feature branches** support iterative development.
* **Pull requests** preserve review and development context.
* **`main`** records accepted logical changes to the repository.

Squash merging provides that separation without requiring contributors to manually curate every intermediate development commit before merge.
