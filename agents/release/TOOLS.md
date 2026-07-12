# TOOLS.md - Release

This role uses only the canonical tool manuals listed here.

Read before acting:

- `../../tools/git.md`
- `../../tools/github-issues.md`
- `../../tools/github-prs.md`
- `../../tools/github-milestones.md`
- `../../tools/github-releases.md`
- `../../tools/local-validation.md`

Do not use tool manuals that are not listed here.

## Authority Summary

Git:

- Maintain the local production-baseline checkout in `rel/main` for support,
  approved hotfix source work, and non-production runtime use.
- Create and update `release/<milestone-name>` branches.
- Merge QA-approved feature branches into release branches.
- Promote validated release branches to `main` in source control.

GitHub Issues:

- Create a release tracking issue at Technical Lead request.
- Read active Milestone issue scope and Product Owner release-content
  decisions.
- Verify issues have QA evidence before release assembly.
- Read release-impacting architecture notes when relevant.
- Comment with release inclusion, promotion, and smoke-test evidence.
- Mark released issues `done` after production promotion.

GitHub Pull Requests:

- Inspect release-related pull requests.
- Use the approved project merge process when applicable.

GitHub Milestones:

- Create or select one active planning milestone at Technical Lead request.
- Read active release Milestone scope.
- Verify included issues match the active Milestone and the Product
  Owner release-content decisions applied by the Technical Lead.
- Do not assign feature or defect issues to the milestone unless the Technical
  Lead explicitly requests that assignment.

GitHub Releases:

- Create GitHub Releases and tags after production promotion.
- View release history.

Local validation:

- Run release-level smoke checks and product-specific release validation.

## Forbidden Operations

- Do not implement feature code.
- Do not include issues outside the active Milestone without Product Owner
  release-content decision, Technical Lead manifest update, and operator
  approval when the manifest is already frozen.
- Do not promote without release QA evidence.
- Do not ignore release-impacting architecture notes.
- Do not create GitHub Releases or tags for planning records.
- Do not create GitHub Releases or tags before production promotion.
- Do not deploy to actual production without the operator-provided production
  folder or runtime location.
- Do not create non-production or production credentials, TLS material,
  service identities, secret roots, service-manager units, or runtime roots.
- Do not delete `dev/**` or `tst/**` checkouts.
- Do not use global credentials when product-scoped credentials exist.
- Do not store secrets in release notes, issues, comments, or local files.

## Required Release Evidence

For each release, record:

- Milestone name.
- Release branch.
- Included issues.
- Deferred issues.
- Release QA evidence.
- Migration, deployment, compatibility, rollback, or operational notes when
  relevant.
- Technical Lead-approved non-production environment material used for smoke
  or deployment validation.
- Operator-provided production folder or runtime location and
  Operations-approved production environment material when actual production
  deployment is in scope.
- Classified readiness findings: `release-blocking`, `operator-waivable`, or
  `follow-up`.
- Operator waiver record and follow-up issue references for every
  `operator-waivable` readiness finding.
- Production promotion commit SHA.
- GitHub Release tag.
- Smoke check result.
- Release-owned `rel/**` cleanup completed, plus `dev/**` and `tst/**`
  cleanup candidates handed off to Technical Lead.
