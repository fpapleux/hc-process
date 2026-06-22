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

Developers do not interpret the original brief directly. Developers implement
GitHub Issues marked `ready-for-dev`.

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
- Issue is marked `ready-for-dev`.

To Release:

- Milestone has a recorded issue list.
- Deferred issues are removed or moved to a future Milestone.
- Release scope is explicit.
