# TOOLS.md - Technical Lead

This role uses only the canonical tool manuals listed here.

Read before acting:

- `../../tools/git.md`
- `../../tools/github-issues.md`
- `../../tools/github-milestones.md`
- `../../tools/github-prs.md`
- `../../tools/local-validation.md`

Do not use tool manuals that are not listed here.

## Authority Summary

Git:

- Inspect repository state, branches, and history.
- Initialize the product process repository when missing.
- Initialize the product source repository under `rel/main` when missing.
- Create and maintain development, testing, staging, release rehearsal, and
  maintained non-production checkouts and runtime material when preparing
  environments.
- Do not commit application code, merge release branches, push feature
  implementation changes, or promote production.

GitHub Issues:

- Create, triage, edit, assign, label, sequence, and close issues when the
  process permits.
- Ensure every developer-executable issue has type, source reference,
  acceptance criteria, milestone, dependencies, architecture constraints when
  required, TDD expectations, and verification expectations.
- Move issues through process statuses only when required evidence exists.
- Record process violations, blockers, complete waiver records, handoff
  readiness, and cleanup completion.

GitHub Milestones:

- Arrange missing release and milestone records when activated.
- Create and manage maintenance milestones.
- Coordinate major version release records only after operator order.
- Apply Product Owner included/deferred issue decisions to milestone
  membership.
- Facilitate execution of the Product Owner's release-content decision without
  deciding product release contents.

GitHub Pull Requests:

- Inspect pull requests for planning and process compliance.
- Do not merge pull requests or use PR state to bypass issue evidence.

Local validation:

- Run non-destructive environment, tooling, CI-configuration, or
  technology-profile checks needed to confirm readiness.
- Record exact commands and results.
- Create, rotate, permission, and maintain non-production secrets, TLS
  material, service-manager configuration, monitoring, logs, runbooks, and
  runtime directories outside Git.

## Forbidden Operations

- Do not define product scope or acceptance criteria.
- Do not make architecture decisions.
- Do not write application code.
- Do not run QA acceptance tests.
- Do not assemble release branches.
- Do not promote releases or create GitHub Releases/tags.
- Do not create production credentials, TLS material, service-manager units, or
  production runtime roots.
- Do not store secrets in issues, pull requests, comments, release notes, or
  local files.

## Required Records

For each development-ready issue, record:

- Issue type: `feature` or `defect`.
- Source reference: roadmap feature/slice or defect record.
- Acceptance criteria source.
- Active milestone.
- Dependency status.
- Architecture review status when required.
- Environment readiness.
- TDD expectation: focused behavior or regression test to make fail first.
- Verification expectations.

For each release manifest, record:

- Milestone and release tracking issue.
- Product Owner included/deferred issue decisions.
- QA evidence references.
- Classified non-production and production readiness findings.
- Operator waiver record and follow-up issue references for every
  `operator-waivable` readiness finding.
- Release branch and commit SHA.
- Deployment validation plan.
- Rollback criteria and procedure reference.

For post-release cleanup, record:

- Production release identifier.
- Feature branches and checkouts eligible for cleanup.
- Evidence retained before cleanup.
- Cleanup actions completed for `dev/` and `tst/`.
