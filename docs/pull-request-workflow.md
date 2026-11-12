# Pull Request Workflow

This workflow applies to repositories in the Land Foundry GitHub organization. It implements the [Source Control & Change History Governance](source-control-change-history-governance.md), which is the source of authority for branching, commits, PR context, and merging. See the [repository governance standard](repository-governance.md) for required controls and exceptions.

This revision is proposed alongside the replacement governance policy. It takes effect upon approval and merge of that proposal. [ADR-001](adr/001-squash-merges.md) records the superseded squash-merge decision for historical reference.

## Short-Lived Branches

Use trunk-based development with short-lived PR branches and `main` as the authoritative integration branch.

1. Create a narrowly scoped branch from the latest `main`.
2. Name it using the policy convention, `<type>/<issue-id>-<short-description>`, when an issue is available.
3. Implement a small, cohesive engineering outcome and run applicable validation.
4. Open a PR targeting `main` and complete automated and human review.
5. Curate the retained commits and revalidate the resulting branch.
6. Merge using a merge commit after all required gates are satisfied.
7. Delete the merged feature branch.

Integrate frequently, ideally within a working day when practical. Decompose larger work into independently mergeable increments. Keep `main` releasable; isolate incomplete functionality safely with feature flags or equivalent mechanisms.

## Meaningful Feature Branch Commits

Each retained commit must represent an intentional, cohesive engineering change with an accurate Conventional Commit message:

```text
feat(auth): introduce token validation interface
feat(auth): implement JWT signature verification
test(auth): cover expired and malformed tokens
```

Include a body when needed to explain rationale, constraints, tradeoffs, or related issues and ADRs. Each commit should be independently buildable and testable where practical; the complete PR must pass all required gates.

Development checkpoints may be used while working. Before integration, review commit boundaries and amend, fix up, or consolidate checkpoints that do not add meaningful engineering context. Messages such as `fix lint`, `fix test again`, and `address review comment` describe activity and should not remain as standalone permanent commits without an independently meaningful engineering change.

Corrections to previously integrated behavior should generally remain distinct `fix` commits. A correction made during development may also remain separate when it provides useful, independent engineering context.

Coordinate any rewriting of shared feature-branch history with contributors and rerun applicable validation. Never rewrite commits already integrated into `main` through routine development workflows. Remove temporary debugging artifacts and unrelated changes.

## Context-Rich Pull Requests

A PR represents a cohesive engineering outcome; its commits explain intentional implementation changes. Keep PRs focused and small enough for effective review, with a clear title describing the result.

Every PR must include:

- **Problem or objective:** The problem being solved or capability introduced.
- **Solution summary:** The selected approach and resulting behavior or structure.
- **Engineering rationale:** Why the approach was selected, including significant constraints and tradeoffs.
- **Validation:** Tests, automated checks, and relevant manual verification actually performed.
- **Impact and risk:** Material compatibility, security, operational, migration, or deployment considerations; state when none were identified.
- **References:** Relevant issues, ADRs, documentation, and dependent PRs, when available.

Add alternatives, rollout plans, observability, configuration changes, migration details, and recovery procedures when relevant. Update the description to reflect the final implementation and keep it understandable after the branch is deleted. Do not report unverified checks as successful.

## Review and Integration Gates

Before merging:

- Obtain all required approvals, including engineer approval for agent-authored changes.
- Pass required CI checks and satisfy the repository's validation requirements against current `main`.
- Resolve review conversations with documented conclusions.
- Confirm the PR description reflects the accepted implementation.
- Confirm retained commits satisfy the policy's commit integrity requirements.
- Remove temporary changes and move unrelated work to separate PRs.
- Renew approvals and validation as required after history curation or other changes.

Apply the repository governance baseline, including stale-approval dismissal and additional approvals where required. Agents follow the same controls as human contributors and must not bypass required quality gates. Reviewers evaluate implementation correctness and the quality of the permanent engineering record.

## Merge Commits and Main Branch History

Use GitHub's **Create a merge commit** option as the standard integration method. The merge commit identifies the PR integration boundary and preserves the curated implementation commits.

For example:

```text
          D---E---F
         /       \
A---B---C-----------M
```

Here, `D`, `E`, and `F` are meaningful engineering commits and `M` integrates the reviewed PR. Associate the merge commit with the PR so readers can move between the outcome, rationale, and individual changes.

Repositories should disable squash merging and rebase merging when enforcing this policy unless an approved, documented exception applies. Do not require linear history on branches using merge commits. Maintain the other protected-branch controls, including PR review, required checks, conversation resolution, and restrictions on force pushes and deletion.

Adoption requires maintainers to review and align repository and organization rulesets and merge-method settings. This workflow documents the desired configuration; updating the documentation does not apply those settings.

## Progressive Disclosure for Humans and Agents

Use repository documentation to discover the system, PRs to understand cohesive outcomes and rationale, commits to inspect intentional implementation changes, and code and tests to establish implementation evidence. Link issues and ADRs for requirements and architectural decisions.

Historical change analysis should normally start with the relevant PR and inspect commits as needed. Troubleshooting may begin with logs, tests, code, deployment records, or `git blame`. Verify historical intent against current code and tests, and distinguish proposed changes, accepted decisions, observed behavior, and hypotheses.

## Releases and Recovery

Merging into `main` is independent of production deployment. Release tags may identify approved versions but are not required for every integration.

Use immutable, traceable artifacts and deployment records that identify their source commits and build executions. Support redeployment of a previously validated artifact where feasible, with separate recovery strategies for changes such as irreversible database migrations. See the governance policy for the full release and recovery requirements.
