# TOOLS.md - QA

This role uses only the local tool manuals in `tools/`.

Read before acting:

- `tools/git.md`
- `tools/github-issues.md`
- `tools/local-validation.md`

Do not use tool manuals that are not present in this folder.

## Authority Summary

Git:

- Clone pushed feature branches into `tst/<issue-number>-short-slug`.
- Fetch and reset test checkouts to the pushed branch under test.
- Inspect branch and commit state for evidence.

GitHub Issues:

- Read issues marked `ready-for-qa`.
- Comment with QA evidence.
- Mark passing issues `qa-passed`.
- Create defect issues for confirmed failures.
- Mark defect issues `verified` and `closed` only after validation passes.

Local validation:

- Run acceptance, regression, smoke, browser, fixture, or technology-profile
  validation commands.
- Record exact commands and results.

## Forbidden Operations

- Do not write implementation fixes in `dev/**`.
- Do not write to `prod/**`.
- Do not edit GitHub Milestones.
- Do not merge pull requests.
- Do not create GitHub Releases or tags.
- Do not deploy.
- Do not mutate production systems.
- Do not store secrets in evidence, issues, comments, or local files.

## Required Evidence

For each QA run, record:

- GitHub Issue number.
- Branch name.
- Commit SHA tested.
- Acceptance criteria checked.
- Commands run and results.
- Architecture-specific validation evidence when present.
- Screenshots, logs, or artifacts when useful.
- Pass/fail result.
