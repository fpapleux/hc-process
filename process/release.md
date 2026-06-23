# Release Process

Release agents assemble tested work into a release branch, promote validated
release branches to production, and create GitHub Releases and tags after
production promotion.

## Intake Requirements

Release starts assembly only when:

- A GitHub Milestone is active.
- Included issues are assigned to the active Milestone.
- Included branches have passing feature QA.
- Release-impacting architecture notes are recorded for included issues when
  relevant.
- Release scope is recorded.

## Assembling a Release

Individual feature QA is required, but it is not sufficient for production
promotion. The assembled release branch gets its own QA pass.

Release assembly rules:

- The release branch is named `release/<milestone-name>`.
- Only issues assigned to the active Milestone are eligible for the release.
- Only branches with passing feature QA are eligible for the release branch.
- Deferred issues are removed from the Milestone or moved to a future Milestone.
- The Release agent records the included issue list before release QA starts.
- Migration, deployment, compatibility, rollback, and operational notes from
  architecture review are carried into release QA and promotion notes.

Create the release branch:

```bash
cd prod/main
git fetch origin
git checkout main
git pull --ff-only origin main
git switch -c release/2026.06.1
```

For an existing release branch, update the local branch from the remote:

```bash
cd prod/main
git fetch origin
git switch release/2026.06.1
git reset --hard origin/release/2026.06.1
```

Merge approved feature branches into the release branch through the approved
GitHub process:

```bash
git merge --no-ff origin/123-checkout-banner
git push -u origin release/2026.06.1
```

## Release QA

QA creates a release test checkout:

```bash
git clone git@github.com:OWNER/REPO.git tst/release-2026.06.1
cd tst/release-2026.06.1
git switch -c release/2026.06.1 --track origin/release/2026.06.1
```

QA validates the assembled release and records release-level evidence in the
Milestone or release tracking issue.

## Production Promotion

Release agents promote the validated release branch to production.

Expected release steps:

```bash
cd prod/main
git fetch origin
git checkout main
git pull --ff-only origin main
git merge --ff-only origin/release/2026.06.1
git push origin main
```

After production promotion, the Release agent creates the GitHub Release and
tag:

```text
Tag: v2026.06.1
Release title: 2026.06.1
Included issues: #123, #124, #125
Validation evidence: release QA run, smoke test output, deployment check
```

## Cleanup

After release, clean up completed phase checkouts:

```bash
rm -rf tst/release-2026.06.1
rm -rf tst/123-checkout-banner
rm -rf dev/123-checkout-banner
```

Delete the remote feature branch only through the approved GitHub or release
process after it has been merged.

## Handoff to Operator

Release reports:

- Release tag.
- Included issues.
- Excluded or deferred issues.
- Validation evidence.
- Production promotion result.
- Smoke check result.
