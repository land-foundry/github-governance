## 1. Purpose

This standard defines the minimum governance controls applied to repositories within the Land Foundry GitHub organization.

The [Source Control & Change History Governance](source-control-change-history-governance.md) defines how engineering changes and their context are structured and retained. The [pull request workflow](pull-request-workflow.md) applies that policy. This standard defines the protection baseline and additional controls by repository classification; documentation-only repositories do not inherit software-specific checks merely by following the change-history workflow.

The objectives are to:

* protect important repository history and content;
* ensure changes are reviewed before integration;
* prevent destructive or untraceable changes;
* establish consistent governance across different repository types;
* apply additional software-quality controls where source code is present;
* allow repository-specific controls to be added without unnecessarily burdening repositories that do not contain executable source code.

The organization follows a layered governance model:

1. **Organization baseline controls** apply to all repositories.
2. **Source code controls** apply to repositories containing executable software.
3. Repositories may define additional controls appropriate to their purpose.

---

# 2. Repository Classification

Repositories are classified according to their primary purpose.

## 2.1 General Repositories

General repositories include repositories whose primary purpose is not executable software.

Examples include:

* organizational documentation;
* architecture documentation;
* backlog and planning repositories;
* policy repositories;
* templates;
* examples;
* research;
* project documentation.

These repositories are governed by the organization baseline.

## 2.2 Source Code Repositories

Source code repositories contain executable software or infrastructure code for which automated validation is appropriate.

Examples include:

* applications;
* APIs;
* services;
* command-line tools;
* reusable libraries;
* platform tooling;
* infrastructure-as-code repositories.

Source code repositories inherit the organization baseline and apply the additional source code controls defined in this standard.

---

# 3. Organization Baseline

The following controls apply to protected default branches across Land Foundry repositories.

## 3.1 Restrict Deletions

Protected branches must not be deleted through normal repository workflows.

This control protects important branches from accidental or unauthorized removal and preserves repository continuity.

---

## 3.2 Require Pull Requests Before Merging

Direct changes to protected branches are not permitted through the normal developer workflow.

Changes must be proposed through a pull request before being integrated into the protected branch.

Pull requests provide a controlled point for:

* review;
* discussion;
* automated validation;
* traceability;
* approval.

---

## 3.3 Require One Approval

A pull request must receive at least **one approval** before it can be merged into a protected branch.

The intent is to maintain a minimum separation between authorship and acceptance of a change.

Where repository staffing or ownership makes independent review impractical, authorized administrative bypasses may be used deliberately rather than weakening the organizational baseline.

---

## 3.4 Dismiss Stale Pull Request Approvals

Existing approvals must be dismissed when new commits are pushed to an approved pull request.

An approval applies to the code or content that was reviewed. Material changes after approval require the resulting revision to be reviewed again.

The intended flow is:

```text
Change proposed
      ↓
Review performed
      ↓
Approval granted
      ↓
New commit pushed
      ↓
Previous approval dismissed
      ↓
Updated change reviewed
```

---

## 3.5 Require Conversation Resolution

All review conversations must be resolved before a pull request can be merged.

This ensures that identified questions, requested changes, and review concerns are explicitly addressed rather than implicitly ignored during merge.

Resolution does not necessarily mean that the reviewer-requested implementation must be adopted. It means that the discussion has reached a documented conclusion.

---

## 3.6 Require Additional Approval for Unattributed Copilot Pull Requests

Pull requests created through GitHub Copilot that are not attributed to a human author require an additional approval before merging.

This creates additional human oversight for changes produced autonomously by an AI coding agent.

The control establishes the principle that greater automation requires corresponding review controls.

Human-authored pull requests:

```text
1 required approval
```

Applicable unattributed Copilot pull requests:

```text
2 required approvals
```

AI-generated changes remain subject to all other repository protections and automated validation requirements.

---

## 3.7 Block Force Pushes

Force pushes to protected branches are prohibited.

Changes must preserve normal Git history and must not rewrite commits already integrated into protected branches.

This protects:

* repository history;
* auditability;
* commit references;
* downstream automation;
* collaborative workflows.

---

## 3.8 Preserve Meaningful Change History

Apply the source control policy's short-lived PR branches, curated Conventional Commits, contextual PR descriptions, and standard merge-commit integration. This governance repository follows the same workflow. Document approved merge-strategy exceptions under the policy.

When enforcing merge commits, allow merge commits and do not require linear history. Squash and rebase merging should be disabled unless an approved exception applies. Preserve the existing approval, conversation-resolution, force-push, and deletion controls.

Use deterministic automation for objective requirements and human review for meaningful commit boundaries, scope, rationale, and architectural judgment. Maintainers must verify actual rulesets and merge settings separately; policy text is not evidence that controls are configured.

---

# 4. Source Code Repository Controls

Repositories classified as source code repositories apply the organization baseline plus the following controls.

## 4.1 Require Status Checks to Pass

Required automated validation must complete successfully before a pull request can merge.

At minimum, source code repositories must provide the following status checks where applicable:

### Lint

```text
lint
```

The lint check verifies adherence to automated syntax, formatting, and static-analysis rules established for the repository's language and toolchain.

Examples may include:

* Ruff;
* ESLint;
* golangci-lint;
* language-specific compiler or formatter validation.

---

### Unit Tests

```text
unit-test
```

The unit-test check validates expected application behavior through automated unit-level testing.

A failing required unit test prevents the change from being merged.

---

### Architecture Tests

```text
architecture-test
```

Architecture tests enforce structural constraints that protect the intended design of the software.

Examples include:

* dependency direction;
* layer boundaries;
* prohibited imports;
* module ownership;
* package relationships;
* architectural fitness functions;
* repository-specific structural invariants.

Architecture tests are intended to make architectural decisions executable where practical rather than relying exclusively on documentation and reviewer knowledge.

---

## 4.2 Require Code Scanning Results

Source code repositories must require acceptable code-scanning results before changes can be merged.

Code scanning analyzes source code for security-relevant weaknesses and vulnerabilities.

The repository's ruleset establishes the accepted severity threshold. Pull requests containing findings at or above that threshold are prevented from merging until the findings are remediated or otherwise handled according to the repository's security process.

The scanner may vary according to repository technology, but the resulting control must be represented through GitHub Code Scanning.

---

## 4.3 Require Code Quality Results

Source code repositories must require acceptable code-quality analysis results before changes can be merged.

Code-quality analysis provides an additional automated signal covering maintainability, correctness, and other detectable quality concerns.

Repository maintainers may configure severity thresholds appropriate to the language, application type, and maturity of the project.

This control complements rather than replaces:

* linting;
* testing;
* architecture tests;
* peer review.

---

# 5. Control Hierarchy

Land Foundry repository governance follows the following hierarchy:

```text
Land Foundry Organization
│
├── Organization Baseline
│   ├── Restrict deletions
│   ├── Require pull requests
│   ├── 1 required approval
│   ├── Dismiss stale approvals
│   ├── Require conversation resolution
│   ├── Additional approval for applicable Copilot PRs
│   └── Block force pushes
│
└── Source Code Repository Policy
    │
    ├── inherits Organization Baseline
    │
    ├── Require status checks
    │   ├── lint
    │   ├── unit-test
    │   └── architecture-test
    │
    ├── Require code scanning results
    └── Require code quality results
```

Additional repository-specific rules may be layered on top of these controls.

For example, an infrastructure repository could additionally require:

```text
terraform-fmt
terraform-validate
terraform-plan
```

while a documentation repository may require no automated status checks at all.

---

# 6. Governance Principles

## 6.1 Baseline Controls Should Be Universally Applicable

Organization-level controls should represent protections that make sense regardless of repository content.

Controls specific to executable software belong in the source code policy rather than the universal organizational baseline.

---

## 6.2 Automation Produces Evidence; Policy Determines Acceptance

CI/CD workflows, security scanners, code-quality systems, tests, and other tools produce evidence about a proposed change.

Repository rulesets define which evidence is required before the change can be accepted.

Conceptually:

```text
Pull Request
     ↓
Automated and human evaluation
     ↓
Evidence
├── reviewer approval
├── lint
├── tests
├── architecture tests
├── security analysis
└── quality analysis
     ↓
Repository governance policy
     ↓
Merge allowed or blocked
```

---

## 6.3 Controls Should Match Repository Risk

Not every repository requires identical technical validation.

A documentation repository should not be required to execute source-code scans simply to satisfy a universal policy.

Conversely, repositories containing executable software require additional engineering controls because changes can introduce defects, vulnerabilities, and architectural degradation.

---

## 6.4 Human Accountability Remains Required

Automation and AI assistance may contribute to repository changes, analysis, and review, but responsibility for accepting changes remains with authorized human contributors.

Additional review requirements may therefore be applied to changes created autonomously by AI systems.

---

# 7. Exceptions and Bypass

Administrative bypasses should be treated as exceptions rather than normal development workflow.

When practical, bypasses should be limited to situations such as:

* recovering from a broken repository configuration;
* repairing failed governance automation;
* resolving an operational incident;
* bootstrapping controls that cannot otherwise be established.

Routine development should continue through the normal protected-branch and pull-request process.

---

# 8. Future Controls

This standard establishes the initial Land Foundry repository-governance baseline.

Additional controls may be introduced as the platform matures, including:

* CODEOWNERS-based approval;
* dependency vulnerability policies;
* secret scanning;
* signed commits or ar
