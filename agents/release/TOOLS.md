# TOOLS.md - Release

This role uses only the local tool manuals in `tools/`.

Read before acting:

- `tools/git.md`
- `tools/github-issues.md`
- `tools/github-prs.md`
- `tools/github-milestones.md`
- `tools/github-releases.md`
- `tools/local-validation.md`

Do not use tool manuals that are not present in this folder.

## Authority Summary

Git:

- Maintain `prod/main`.
- Create and update `release/<milestone-name>` branches.
- Merge QA-approved feature branches into release branches.
- Promote validated release branches to `main`.

GitHub Issues:

- Read active Milestone issue scope.
- Verify issues have QA evidence before release assembly.
- Comment with release inclusion, promotion, and smoke-test evidence.
- Mark released issues `done` after production promotion.

GitHub Pull Requests:

- Inspect release-related pull requests.
- Use the approved project merge process when applicable.

GitHub Milestones:

- Read active release Milestone scope.
- Verify included issues match the active Milestone.

GitHub Releases:

- Create GitHub Releases and tags after production promotion.
- View release history.

Local validation:

- Run release-level smoke checks and product-specific release validation.

## Forbidden Operations

- Do not implement feature code.
- Do not include issues outside the active Milestone without Product Owner
  approval.
- Do not promote without release QA evidence.
- Do not create GitHub Releases or tags before production promotion.
- Do not use global credentials when product-scoped credentials exist.
- Do not store secrets in release notes, issues, comments, or local files.

## Required Release Evidence

For each release, record:

- Milestone name.
- Release branch.
- Included issues.
- Deferred issues.
- Release QA evidence.
- Production promotion commit SHA.
- GitHub Release tag.
- Smoke check result.
