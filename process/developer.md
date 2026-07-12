# Developer Process

The Developer implements assigned GitHub Issues marked `ready-for-dev`.
Development work happens only in `dev/**`.

## Intake Requirements

The Developer starts work only from a GitHub Issue that is:

- Assigned to the active release Milestone.
- Marked `ready-for-dev`.
- Typed as `feature` or `defect`.
- Tied to a source roadmap feature or defect record.
- Clear enough to implement.
- Paired with acceptance criteria.
- Paired with a product profile.
- Paired with a technology profile.
- Paired with architecture review evidence when the issue is technically
  significant.

If the issue is unclear, missing acceptance criteria, missing a source roadmap
feature or defect record, missing a technology profile, missing required
architecture review, or expanding beyond its scope, return it to the Technical
Lead instead of guessing.

## Branch and Checkout Naming

Use the GitHub Issue number in local folder names and branch names:

```text
dev/<issue-number>-short-slug
branch: <issue-number>-short-slug
```

Example:

```text
dev/123-checkout-banner
branch: 123-checkout-banner
```

## Starting Work

Create a development checkout from the remote production baseline (`main`):

```bash
git clone git@github.com:OWNER/REPO.git dev/123-checkout-banner
cd dev/123-checkout-banner
git switch -c 123-checkout-banner origin/main
```

Before implementation starts:

- Verify the GitHub Issue is assigned to the active release Milestone.
- Verify the issue is marked `ready-for-dev`.
- Verify the issue is typed as `feature` or `defect` and tied to a source
  roadmap feature or defect record.
- Move the issue to `in-dev`.
- Confirm the product profile.
- Confirm the technology profile.
- Confirm architecture notes and constraints when the issue is marked
  `architecture-reviewed`.

## Implementation Workflow

1. Read the GitHub Issue, source reference, and acceptance criteria.
2. Confirm whether the issue completes a roadmap feature, completes a slice of
   a roadmap feature, or fixes a defect.
3. Inspect the existing codebase before changing it.
4. Implement the scoped change.
5. Add or update code-level tests.
6. Run relevant technology-profile checks.
7. Commit with the GitHub Issue number.
8. Push the feature branch.
9. Verify that CI has started for the pushed branch when the repository has CI.
10. Record implementation notes and verification evidence in the GitHub Issue or
    pull request.
11. Mark the issue `ready-for-qa`.

Example:

```bash
cd dev/123-checkout-banner
# edit code
# run relevant tests
git status
git add .
git commit -m "Implement checkout banner for #123"
git push -u origin 123-checkout-banner
```

## Verification Evidence

The Developer records exact verification before handoff.

Example:

```text
Verification:
- npm run test: passed
- npm run build: passed
- manual check: not run, reason: no browser behavior changed
```

If a relevant check was not run, record the reason.

When the repository has CI, Developer evidence must also include:

- pushed branch name;
- pushed commit SHA;
- CI workflow/run identifier or URL for that exact commit;
- CI status, or a clear blocker if CI did not start.

If CI does not start for the pushed branch, do not mark the issue
`ready-for-qa` unless the Technical Lead explicitly records a waiver record in
the issue following the Waiver Records section of `overview.md`. Missing CI
caused by a workflow trigger that only names specific feature branches is a
process defect and must be fixed before normal development continues.

## Pull Request Rules

- Developer can create or update a draft pull request when the product process
  requires one.
- Developer does not merge pull requests.
- Developer does not decide release inclusion.
- Developer records the PR link in the GitHub Issue when a PR exists.

## Stop Conditions

Stop and return to Product Owner when the work requires:

- Scope expansion.
- Changed acceptance criteria.
- Missing or conflicting requirements.
- A missing technology profile.
- Missing required architecture review.
- Schema or data migration not described in the issue.
- Production credentials.
- Release timing changes.
- Production system mutation.

Stop and request Architect review when the implementation reveals a new system
boundary, dependency, migration, security, reliability, performance, or
operational concern not covered by the issue.

## Defect Fixes

Developer fixes confirmed QA defects in `dev/<feature-branch>`.

Required defect loop:

1. Accept the GitHub Issue and become the owner.
2. Move the issue to `accepted`.
3. Fix the bug in `dev/**`.
4. Add or update the regression test.
5. Commit and push with the GitHub Issue number.
6. Record the fix commit SHA and tests run in the GitHub Issue.
7. Move the issue to `fixed`.

Example:

```bash
cd dev/123-checkout-banner
# fix bug
# add/update regression test
git add .
git commit -m "Fix #456 in checkout banner"
git push
```

## Handoff to QA

Handoff requires:

- GitHub Issue number.
- Pushed feature branch.
- Commit SHA.
- Verification commands and results.
- Known limitations.
- Pull request link when one exists.
