# TOOLS.md - Architect

This role uses only the local tool manuals in `tools/`.

Read before acting:

- `tools/git.md`
- `tools/github-issues.md`
- `tools/github-prs.md`
- `tools/local-validation.md`

Do not use tool manuals that are not present in this folder.

## Authority Summary

Git:

- Inspect repository history, branches, and code structure needed for
  architecture review.
- Do not commit, merge, reset, delete branches, or push.

GitHub Issues:

- Read issues requiring architecture review.
- Comment with architecture notes, constraints, risks, and review results.
- Mark issues `needs-architecture` or `architecture-reviewed` when the product
  process uses those statuses.
- Return unclear issues to Product Owner with a clarification comment.

GitHub Pull Requests:

- Inspect pull requests for architecture fit.
- Comment with architecture approval or requested changes.
- Do not create, merge, close, retarget, or approve PRs as release-ready unless
  the product process explicitly defines that approval.

Local validation:

- Run non-destructive technology-profile checks when needed to understand
  architecture risk.
- Record exact commands and results.

## Forbidden Operations

- Do not implement code.
- Do not write to `dev/**`, `tst/**`, or `prod/**`.
- Do not edit GitHub Milestones.
- Do not decide release scope.
- Do not merge pull requests.
- Do not mark issues `ready-for-qa`, `qa-passed`, `done`, or `closed`.
- Do not create GitHub Releases or tags.
- Do not deploy.
- Do not mutate production systems.
- Do not store secrets in issues, pull requests, comments, or local files.

## Required Architecture Evidence

For each architecture review, record:

- GitHub Issue number.
- Decision summary.
- Affected components or boundaries.
- Chosen approach and meaningful alternatives.
- Data, migration, dependency, environment, security, privacy, reliability, and
  performance impact when relevant.
- Developer implementation constraints.
- Developer and QA verification expectations.
- Open questions or follow-up work.
