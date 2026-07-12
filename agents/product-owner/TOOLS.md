# TOOLS.md - Product Owner

This role uses only the canonical tool manuals listed here.

Read before acting:

- `../../tools/github-issues.md`
- `../../tools/github-milestones.md`

Do not use tool manuals that are not listed here.

## Authority Summary

GitHub Issues:

- Create feature issues.
- Create defect issues.
- Edit issue body, labels, owner, status, priority, release intent, and
  included/deferred release decisions.
- Comment on issues to record scope decisions.
- Mark technically significant issues `needs-architecture`.
- Do not mark issues `ready-for-dev`; record the product information needed
  for Technical Lead readiness review.
- Record release decisions such as `included` or `deferred` for Product Owner
  scope purposes.

GitHub Milestones:

- Read planning Milestones to understand release targets.
- Record intended milestone names, release priority input, and included/deferred
  issue decisions.
- Do not create or edit planning Milestones.
- Do not assign issues to, remove issues from, or move issues between
  Milestones. The Technical Lead applies Product Owner release-content
  decisions to milestone membership.

## Forbidden Operations

- Do not implement code.
- Do not create pull requests.
- Do not merge pull requests.
- Do not create GitHub Releases or tags.
- Do not deploy.
- Do not mutate production systems.
- Do not store secrets in issues, comments, or local files.

## Required Records

For each issue created from a brief, record:

- Issue type: `feature` or `defect`.
- Source reference: roadmap phase and feature name or ID for features; defect
  report, QA failure, incident, or operator report for defects.
- User goal or business reason.
- Scope.
- Out-of-scope notes.
- Acceptance criteria.
- Dependencies.
- Intended release target or Milestone.
- Product priority.
- Release decision: included, deferred, or not yet decided.
- Status.
- Technology label when known.
- Architecture review requirement when the issue is technically significant.
