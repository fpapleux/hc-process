# TOOLS.md - Developer

This role uses only the canonical tool manuals listed here.

Read before acting:

- `../../tools/git.md`
- `../../tools/github-issues.md`
- `../../tools/github-prs.md`
- `../../tools/local-validation.md`

Do not use tool manuals that are not listed here.

## Authority Summary

Git:

- Clone the repository into `dev/<issue-number>-short-slug`.
- Create feature branches from `origin/main`.
- Commit scoped implementation changes.
- Push feature branches.

GitHub Issues:

- Read assigned issues and architecture notes.
- Comment on assigned issues.
- Move assigned issues from `ready-for-dev` to `in-dev`.
- Move implemented issues to `ready-for-qa`.
- Record implementation notes, commit SHA, and verification evidence.

GitHub Pull Requests:

- Create draft pull requests when the product process requires one.
- Update draft pull requests with implementation and verification notes.
- Comment on pull requests for implementation context.

Local validation:

- Run technology-profile test, lint, typecheck, build, or verification commands.
- Run the focused test before production code changes and record the expected
  failure, then rerun after implementation and record the passing result.
- Record exact commands and results before QA handoff.

## Forbidden Operations

- Do not write to `tst/**`.
- Do not write to `rel/**`.
- Do not edit GitHub Milestones.
- Do not merge pull requests.
- Do not close issues.
- Do not create GitHub Releases or tags.
- Do not deploy.
- Do not mutate production systems.
- Do not store secrets in commits, issues, pull requests, or local files.

## Required Handoff Evidence

Before moving an issue to `ready-for-qa`, record:

- GitHub Issue number.
- Branch name.
- Commit SHA.
- Pull request link when one exists.
- TDD evidence: focused test added or updated, expected failing run, and
  passing rerun after implementation.
- Verification commands and results.
- CI run evidence for the pushed commit, or a Technical Lead waiver record on
  the issue when CI is unavailable.
- Architecture constraints followed when relevant.
- Known limitations.
