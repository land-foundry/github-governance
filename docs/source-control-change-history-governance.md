# Land Foundry — Source Control & Change History Governance

**Version:** 1.0 — Draft  
**Status:** Proposed  
**Scope:** Software engineering repositories  
**Owner:** Engineering

---

## 1. Purpose

This policy establishes standards for branching, commits, pull requests, and merging across Land Foundry repositories.

Its primary objectives are to:

- Support high-quality, engineer-directed agentic software development.
- Enable frequent integration of small, independently understandable changes.
- Maintain a trustworthy, discoverable record of engineering decisions and implementation history.
- Reduce the cognitive effort required for engineers and AI agents to understand, maintain, and troubleshoot software.
- Support continuous integration, reliable delivery, and operational recovery.
- Establish consistent conventions that can be enforced through automation and reused by engineering agents and skills.

Source control history is considered an **engineering knowledge asset**, not merely a record of development activity.

## 2. Governing Principles

### 2.1 Progressive disclosure of engineering context

Engineering information MUST be organized so that humans and AI agents can progressively retrieve more detail as needed.

The intended hierarchy is:

| Level | Artifact | Primary responsibility |
|---|---|---|
| 1 | Repository documentation | Explain the system, its structure, and navigation |
| 2 | Pull request | Describe a cohesive engineering change and its rationale |
| 3 | Commit | Explain a specific, intentional implementation change |
| 4 | Code, tests, and diffs | Demonstrate the actual implementation and behavior |
| Cross-cutting | Issues and ADRs | Preserve requirements, constraints, and significant architectural decisions |

An agent investigating an existing change SHOULD begin with its PR-level description, then inspect relevant commits and implementation details only when necessary.

This is a preferred retrieval strategy, not a mandatory traversal order for every task. Defect localization may appropriately begin with code, tests, logs, or `git blame`.

Information SHOULD be referenced rather than unnecessarily duplicated across levels.

### 2.2 Engineering intent over development activity

Permanent source control history MUST emphasize intentional engineering changes rather than the chronological sequence of implementation attempts.

Intermediate corrections, experimentation, and development checkpoints SHOULD be consolidated before integration when they do not independently contribute meaningful engineering context.

### 2.3 Small, cohesive changes

Changes SHOULD be divided into the smallest practical units that can be implemented, validated, understood, and reviewed independently.

Change scope is evaluated primarily by conceptual complexity, not line count.

Artificially fragmenting tightly coupled changes is discouraged.

### 2.4 Human accountability

AI agents MAY implement changes, generate commits, draft PRs, and propose changes to commit history.

Engineers remain accountable for validating the correctness, intent, quality, and integration readiness of accepted changes.

Automated checks provide supporting evidence but do not replace engineering judgment.

### 2.5 Reliable integration

The primary branch MUST remain in a releasable condition.

Changes MUST satisfy applicable quality gates before integration. Incomplete functionality MAY be integrated when safely isolated through feature flags or equivalent mechanisms.

---

## 3. Branching Strategy

### 3.1 Standard model

Land Foundry adopts **trunk-based development with short-lived pull request branches**.

`main` is the authoritative integration branch.

Developers and agents MUST create short-lived branches for changes requiring PR review.

Long-lived development branches are discouraged.

### 3.2 Branch lifecycle

1. Create a branch from the latest `main`.
2. Implement a narrowly scoped engineering change.
3. Execute applicable local validation.
4. Open a pull request.
5. Complete automated and human review.
6. Curate the branch's commit history.
7. Merge into `main` after satisfying required checks.
8. Delete the merged feature branch.

Changes SHOULD integrate frequently, ideally within a working day when practical. Larger work SHOULD be decomposed into independently mergeable increments where feasible.

### 3.3 Branch naming

Branches SHOULD follow a consistent naming convention:

`<type>/<issue-id>-<short-description>`

Examples:

- `feat/123-authentication-service`
- `fix/145-token-validation`
- `refactor/167-extract-repository-interface`
- `docs/182-update-architecture-guide`

Supported types SHOULD align with the organization's commit taxonomy.

### 3.4 Branch protection

The `main` branch MUST be protected through GitHub rulesets.

At minimum, repositories MUST require:

- Pull requests before integration.
- At least one approving review.
- Passing required CI checks.
- Resolution of review conversations.
- Dismissal of stale approvals when required by repository policy.
- Restrictions on force pushes and deletion.

Administrators and automated identities SHOULD follow the same protections unless an explicitly authorized exception applies.

---

## 4. Commit Strategy

### 4.1 Commit integrity

**A commit represents an intentional unit of engineering change, not a unit of development effort.**

Each commit MUST:

- Represent a cohesive and meaningful change.
- Have an accurate, descriptive message.
- Avoid unrelated modifications.
- Preserve enough information to understand its purpose.
- Avoid introducing known, unresolved defects within the scope of applicable validation.

Each commit SHOULD be independently buildable and testable where practical.

The complete PR MUST pass all required quality gates before integration.

### 4.2 Commit messages

Commit messages MUST follow Conventional Commits conventions.

Format:

`<type>(<scope>): <description>`

Examples:

- `feat(auth): introduce token validation interface`
- `feat(auth): implement JWT signature verification`
- `test(auth): cover expired and malformed tokens`
- `fix(auth): reject tokens with invalid audiences`
- `refactor(api): isolate authentication dependencies`

A commit body SHOULD be provided when the engineering rationale is not obvious.

Commit bodies SHOULD describe:

- Why the change was necessary.
- Important implementation decisions.
- Relevant constraints or tradeoffs.
- References to related issues or ADRs.

The description MUST remain consistent with the actual diff.

### 4.3 Development checkpoints

Intermediate commits MAY be used during implementation.

Examples include:

- Work-in-progress checkpoints.
- Temporary debugging changes.
- Fixes for lint errors introduced by the ongoing change.
- Corrections to newly introduced failing tests.
- Adjustments addressing review feedback.

Such commits SHOULD be amended, fixed up, or consolidated into meaningful engineering commits before integration.

A separate correction commit MAY be retained when the correction itself represents an independently meaningful engineering change.

### 4.4 Defect corrections

Corrections to previously integrated behavior SHOULD generally be preserved as distinct `fix` commits.

This maintains traceability between the previously accepted implementation and the subsequent correction.

### 4.5 Commit history curation

Before merging, the PR author or responsible agent SHOULD review the branch's commits for:

- Logical boundaries.
- Accurate descriptions.
- Unnecessary intermediate commits.
- Unrelated changes.
- Missing engineering rationale.
- Appropriate test coverage.

Interactive rebase, commit amendment, and fixup operations MAY be used on feature branches.

Rewriting shared branch history MUST be coordinated with contributors and MUST trigger applicable revalidation.

Commits already integrated into `main` MUST NOT be rewritten through routine development workflows.

---

## 5. Pull Request Strategy

### 5.1 PR responsibility

**A pull request represents a cohesive engineering outcome.**

While commits describe individual implementation changes, the PR explains the broader problem, proposed solution, rationale, and evidence that the change is ready for integration.

PR descriptions MUST provide enough information for an engineer or AI agent to understand the purpose of the change without first reviewing every commit.

### 5.2 PR scope

Each PR SHOULD:

- Address one clearly defined objective.
- Minimize unrelated modifications.
- Link to its corresponding issue when available.
- Include appropriate automated tests.
- Be small enough for focused engineering review.

Large initiatives SHOULD be divided into smaller, independently understandable PRs where feasible.

### 5.3 Required PR information

PRs MUST include:

**Problem or objective**

What problem is being solved, or what capability is being introduced?

**Solution summary**

What approach was selected, and what behavior or structure changed?

**Engineering rationale**

Why was this solution selected? Include significant constraints and tradeoffs where relevant.

**Validation**

What testing, automated checks, and relevant manual verification were performed?

**Impact and risk**

Identify meaningful compatibility, security, operational, migration, or deployment considerations. State when no material risks were identified.

**References**

Link relevant issues, architectural decisions, documentation, and dependent PRs.

### 5.4 Optional PR information

Depending on complexity, PRs SHOULD also document:

- Alternatives considered.
- Configuration or infrastructure changes.
- Feature flags and rollout approach.
- Observability changes.
- Database migration requirements.
- Compatibility considerations.
- Rollback or recovery procedures.

### 5.5 PR descriptions as agent-accessible context

PR descriptions SHOULD remain understandable after the associated feature branch has been deleted.

Agents consuming historical PR information SHOULD be able to identify:

1. The engineering problem.
2. The accepted solution.
3. The reasons behind the solution.
4. The commits implementing the solution.
5. Any relevant architectural constraints.

PR descriptions SHOULD be updated when the final implementation materially differs from the original proposal.

---

## 6. Merge Strategy

### 6.1 Standard merge method

Land Foundry adopts **merge commits** as its standard integration method.

This approach preserves the original commits and their relationship to the integrated feature branch.

Merge commits provide an identifiable integration boundary, while retaining the individual engineering changes that make up the PR.

### 6.2 Merge requirements

Before merging:

- All required reviews MUST be approved.
- Required CI checks MUST pass.
- Review conversations MUST be resolved.
- The PR description MUST accurately reflect the accepted implementation.
- Commit history MUST satisfy the commit integrity requirements.
- The branch MUST satisfy the repository's current-`main` validation requirements.

The resulting merge commit SHOULD be associated with the corresponding PR.

### 6.3 Merge method restrictions

Repositories SHOULD disable squash merging and rebase merging when enforcing this policy, unless an approved exception exists.

Merge commits MUST NOT be used as a justification for preserving unnecessary implementation checkpoints.

### 6.4 Exceptions

Alternative merge strategies MAY be approved for repositories with distinct lifecycle requirements, such as:

- Documentation-only repositories.
- Experimental or disposable repositories.
- External contribution models.
- Repositories with specialized release or maintenance requirements.

Exceptions MUST be documented.

---

## 7. Release and Deployment Independence

Branching and merging policy MUST remain independent of environment deployment and release management.

Merging into `main` does not inherently require immediate production deployment.

CI/CD SHOULD produce immutable, traceable artifacts associated with the source commit.

Production deployment records SHOULD identify:

- The deployed artifact and immutable identifier.
- Its originating commit.
- The relevant release identifier, where applicable.
- Deployment status and environment.
- The associated build or workflow execution.

Release tags MAY identify approved versions, but are not required for every integration.

Production recovery SHOULD support redeployment of a previously validated artifact where technically feasible.

Changes that cannot be safely rolled back through artifact replacement, such as certain database migrations, MUST have an appropriate recovery strategy.

---

## 8. Agentic Engineering Requirements

### 8.1 Context discovery

Engineering agents SHOULD use a progressive context retrieval strategy.

For repository onboarding, agents SHOULD begin with repository documentation and architecture guidance.

For historical change analysis, agents SHOULD locate relevant PRs and inspect individual commits as needed.

For troubleshooting, agents SHOULD select an appropriate entry point using available evidence, including logs, tests, code history, or deployment records.

### 8.2 Information integrity

Agents MUST distinguish among:

- Proposed versus accepted changes.
- Historical versus current implementation.
- Requirements versus observed behavior.
- Architectural intent versus implementation details.
- Validated conclusions versus hypotheses.

Agents MUST NOT assume historical implementations remain current.

Current source code and executable tests provide evidence of implementation and behavior, while PRs, issues, and ADRs provide additional context about intent.

### 8.3 Agent-generated changes

Agents MUST follow the same branching, commit, PR, and merge requirements as human contributors.

Agents SHOULD:

- Plan meaningful commit boundaries.
- Maintain small, focused changes.
- Run applicable validation.
- Propose descriptive commit messages.
- Draft accurate PR descriptions.
- Identify relevant architectural constraints.
- Report unresolved questions or risks.

Agents MUST NOT bypass required quality gates or represent unverified results as successful.

### 8.4 Human review

An engineer MUST review and approve agent-authored changes under the standard repository protection policy.

Review MUST consider both implementation correctness and the quality of the permanent engineering record.

---

## 9. Governance Enforcement

Governance SHOULD use a combination of deterministic automation and human review.

### 9.1 Automated controls

Examples include:

- Branch naming validation.
- Conventional Commit validation.
- Required PR description sections.
- Required tests and static analysis.
- Branch protection and merge method restrictions.
- Detection of obvious checkpoint-style commit messages.

Automated checks SHOULD enforce objective requirements.

### 9.2 Human judgment

Engineers remain responsible for evaluating:

- Whether commits represent meaningful engineering changes.
- Whether PRs are appropriately scoped.
- Whether rationale is sufficient.
- Whether changes are architecturally sound.
- Whether commit boundaries improve historical understanding.

Automated agents MAY assist with these evaluations, but subjective assessments SHOULD not become rigid blocking controls without demonstrated reliability.

---

## 10. Evaluation and Continuous Improvement

Governance effectiveness SHOULD be evaluated through both delivery performance and engineering usability.

Relevant indicators include:

**Delivery performance**

- Change lead time.
- Deployment frequency.
- Change fail rate.
- Failed deployment recovery time.
- Deployment rework rate.

**Engineering process**

- PR review turnaround time.
- PR size and scope.
- Frequency of integration.
- CI feedback latency.
- History-related review rework.

**Agentic engineering effectiveness**

- Accuracy of historical context retrieval.
- Ability to associate implementation changes with their rationale.
- Successful identification of regressions.
- Correctness of agent-generated modifications.
- Human effort required to validate agent output.

The organization SHOULD periodically reassess whether the selected governance practices improve engineering outcomes without creating disproportionate overhead.

---

## 11. Implementation Artifacts

This policy establishes the foundation for additional reusable engineering capabilities.

Planned artifacts include:

| Artifact | Responsibility |
|---|---|
| `CONTRIBUTING.md` | Contributor-facing workflow and standards |
| PR template | Standardize the engineering change narrative |
| Commit authoring skill | Generate meaningful commit boundaries and messages |
| PR authoring skill | Generate complete, context-rich PR descriptions |
| History curation skill | Inspect and improve commits before integration |
| PR review agent | Evaluate code quality and governance compliance |
| Repository onboarding agent | Discover architecture and engineering history |
| Troubleshooting agent | Use implementation, commit, PR, and deployment history to diagnose defects |
| GitHub Actions checks | Enforce deterministic governance requirements |

These artifacts SHOULD reference this policy as their source of authority rather than duplicating its rules independently.

---

## 12. Policy Summary

Land Foundry adopts the following engineering standards:

- **Branching:** Trunk-based development with short-lived PR branches.
- **Commits:** Curated, meaningful, intentional engineering changes.
- **Commit messages:** Conventional Commits with additional rationale when needed.
- **Pull requests:** Small, cohesive, context-rich descriptions of engineering outcomes.
- **Merging:** Merge commits preserving approved individual commit history.
- **Integration:** Protected `main` with automated validation and engineer approval.
- **Deployment:** Immutable artifacts and explicit release promotion independent of branch structure.
- **Agentic development:** Progressive context discovery with human accountability.
- **Governance:** Automate objective checks and preserve human engineering judgment.

**Foundational principle:**

> Source control should preserve the engineering knowledge necessary to understand not only what changed, but why it changed, how it was implemented, and how it can be safely evolved.
