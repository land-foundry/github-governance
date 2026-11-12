# Land Foundry GitHub Governance

This repository is the source of authority for GitHub governance across Land Foundry. Source control history is an engineering knowledge asset for people and agents.

## Engineering Direction

- Use trunk-based development with short-lived PR branches from `main`.
- Retain curated Conventional Commits that explain intentional engineering changes.
- Write contextual PRs that explain the problem, solution, rationale, validation, and impact.
- Integrate with merge commits to preserve the reviewed implementation commits and PR boundary.
- Keep release and deployment decisions independent of branch integration.
- Automate objective requirements and retain human accountability for acceptance.

## Read the Governance

| Document | Responsibility |
|---|---|
| [Source Control & Change History Governance](docs/source-control-change-history-governance.md) | Authoritative branching, commit, PR, merge, agent, and release principles |
| [Repository governance standard](docs/repository-governance.md) | Organization baseline protections and additional source code controls |
| [Pull request workflow](docs/pull-request-workflow.md) | Contributor procedure for applying the policy |
| [PR template](.github/pull_request_template.md) | Required context for proposed engineering outcomes |

## Discover Engineering Context

Start with repository documentation for onboarding. For a historical change, read its PR to understand the outcome and rationale, then inspect relevant commits, code, and tests. Issues and ADRs preserve requirements and architectural decisions. Troubleshooting may begin wherever the evidence points; always check historical context against the current implementation.

## Architecture Decision Records

- [ADR-001: Use squash merges for pull requests (superseded)](docs/adr/001-squash-merges.md)
- [ADR-002: Use merge commits with curated engineering history](docs/adr/002-merge-commits.md)
- [ADR template](templates/adr.md)

Preserve superseded decisions as historical context and link their replacements.

## Contributing

Follow the [pull request workflow](docs/pull-request-workflow.md), including commit curation and required review. This governance repository follows that workflow too. Changes to policy and changes to GitHub settings are separate actions; documentation does not establish that a control has been configured or verified.
