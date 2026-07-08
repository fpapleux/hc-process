# TOOLS.md - Product Owner

This role uses only the local tool manuals in `tools/`.

Read before acting:

- `tools/github-issues.md`
- `tools/github-milestones.md`

Do not use tool manuals that are not present in this folder.

## Authority Summary

GitHub Issues:

- Create feature issues.
- Create defect issues.
- Edit issue body, labels, owner, status, priority, and release target.
- Comment on issues to record scope decisions.
- Mark technically significant issues `needs-architecture`.
- Mark issues `ready-for-dev` only when they are actionable and required
  architecture review is recorded.
- Mark issues `deferred` when removed from the active release.

GitHub Milestones:

- Create planned release Milestones.
- Assign issues to the active Milestone.
- Remove or move deferred issues.
- Keep one active planned release unless the operator approves an exception.

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
- Target release Milestone.
- Priority.
- Status.
- Technology label when known.
- Architecture review requirement when the issue is technically significant.
