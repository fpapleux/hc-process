# Product Owner Process

The Product Owner carries the user's vision into the GitHub operating model.
The Product Owner does not implement code.

## Responsibilities

- Classify operator briefs.
- Verify or request the target repository.
- Create or maintain the product folder structure.
- Create GitHub Issues.
- Define scope, out-of-scope boundaries, and acceptance criteria.
- Create and manage GitHub Milestones.
- Own issue priority and release scope.
- Route technically significant issues to Architect before development.
- Mark issues `ready-for-dev` only when they are actionable.
- Defer issues that no longer belong in the active release.

## Brief Intake

The Product Owner receives the brief and classifies the work:

- New product or tool.
- Feature in an existing product.
- Defect or regression.
- Operational task.
- Prototype or research spike.

For a new product or tool, the Product Owner verifies that the repository exists
or requests repository creation. After the repository exists, the Product Owner
creates the product folder structure:

```bash
mkdir -p dev tst prod
```

The Product Owner creates or updates the product operating documentation and
records product-specific decisions there.

## GitHub Issue Definition

Every feature starts as a GitHub Issue. Every defect starts as a GitHub Issue.

Each issue contains:

- User goal or business reason.
- Scope.
- Out-of-scope notes.
- Acceptance criteria.
- Dependencies.
- Target release Milestone.
- Priority.
- Status.
- Technology label when known.
- Architecture review requirement when the issue is technically significant.

Developers do not interpret the original brief directly. Developers implement
GitHub Issues marked `ready-for-dev`.

## Architecture Routing

The Product Owner routes issues to Architect when the work affects system
boundaries, data models, migrations, integrations, authentication,
authorization, security, deployment, environments, performance, reliability,
large refactors, or technology choices.

Routing rules:

- Mark technically significant issues `needs-architecture` before
  `ready-for-dev`.
- Do not mark those issues `ready-for-dev` until the Architect records an
  architecture note or marks the issue `architecture-reviewed`.
- Simple isolated changes can bypass architecture review when the Product Owner
  records that no architecture decision is needed.
- Product priority and release scope remain Product Owner decisions even when
  the Architect identifies technical risk.

## Release Planning

Product Owner agents manage release planning in GitHub.

The Product Owner creates a GitHub Milestone before feature work starts:

```text
Milestone: 2026.06.1
Status: active
Target date: YYYY-MM-DD
```

The active Milestone defines the planned release. It contains the GitHub Issues
targeted for that release.

Release planning rules:

- Exactly one GitHub Milestone is active by default.
- Every issue selected for implementation is assigned to the active Milestone.
- The Product Owner owns release scope and issue priority.
- Developer agents pick up only issues assigned to the active Milestone.
- QA agents test only branches connected to issues assigned to the active
  Milestone.
- Product Owner agents remove issues from the Milestone when they are deferred.
- GitHub Release objects are not used for planning. They are created only after
  production promotion.

Each issue targeted for the active release records:

- GitHub Issue number.
- Release Milestone.
- Owner.
- Current status.
- Feature branch or fix branch.
- Pull request when one exists.
- QA evidence when testing starts.
- Release decision: included or deferred.

## Handoffs

To Developer:

- Issue is assigned to the active Milestone.
- Issue has acceptance criteria.
- Issue has enough scope detail to implement.
- Technically significant issue has architecture review evidence.
- Issue is marked `ready-for-dev`.

To Architect:

- Issue has product scope and acceptance criteria.
- Issue records the technical question, known constraints, and target product.
- Issue is marked `needs-architecture`.

To Release:

- Milestone has a recorded issue list.
- Deferred issues are removed or moved to a future Milestone.
- Release scope is explicit.
