# Developer Process

The Developer implements assigned GitHub Issues marked `ready-for-dev`.
Development work happens only in `dev/**`.

## Intake Requirements

The Developer starts work only from a GitHub Issue that is:

- Assigned to the active release Milestone.
- Marked `ready-for-dev`.
- Clear enough to implement.
- Paired with acceptance criteria.
- Paired with a product profile.
- Paired with a technology profile.

If the issue is unclear, missing acceptance criteria, missing a technology
profile, or expanding beyond its scope, return it to the Product Owner instead
of guessing.

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

Create a development checkout from the remote production baseline:

```bash
git clone git@github.com:OWNER/REPO.git dev/123-checkout-banner
cd dev/123-checkout-banner
git switch -c 123-checkout-banner origin/main
```

Before implementation starts:

- Verify the GitHub Issue is assigned to the active release Milestone.
- Verify the issue is marked `ready-for-dev`.
- Move the issue to `in-dev`.
- Confirm the product profile.
- Confirm the technology profile.

## Implementation Workflow

1. Read the GitHub Issue and acceptance criteria.
2. Inspect the existing codebase before changing it.
3. Implement the scoped change.
4. Add or update code-level tests.
5. Run relevant technology-profile checks.
6. Commit with the GitHub Issue number.
7. Push the feature branch.
8. Record implementation notes and verification evidence in the GitHub Issue or
   pull request.
9. Mark the issue `ready-for-qa`.

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
- Schema or data migration not described in the issue.
- Production credentials.
- Release timing changes.
- Production system mutation.

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
