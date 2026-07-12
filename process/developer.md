# Developer Process

The Developer implements assigned GitHub Issues marked `ready-for-dev`.
Development work happens only in `dev/**`.

Developer work is test-driven. For every feature, defect fix, refactor, or
behavior change, write or update the focused automated test before production
code, run it, and confirm the expected failure. Only then implement the minimal
change required to pass. Refactor only after the focused test and relevant
checks are green.

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
4. Identify the smallest behavior increment and the automated test that should
   fail before the change exists.
5. Add or update that focused test.
6. Run the focused test and confirm it fails for the expected reason. If it
   passes immediately or fails for the wrong reason, correct the test before
   changing production code.
7. Implement the minimal scoped change needed to pass the focused test.
8. Run the focused test and relevant technology-profile checks.
9. Refactor only while keeping the focused test and relevant checks green.
10. Commit with the GitHub Issue number.
11. Push the feature branch.
12. Verify that CI has started for the pushed branch when the repository has CI.
13. Record implementation notes and verification evidence in the GitHub Issue or
    pull request.
14. Mark the issue `ready-for-qa`.

Example:

```bash
cd dev/123-checkout-banner
# add/update focused failing test
# run focused test and record expected failure
# implement minimal change
# rerun focused and relevant tests
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
- TDD red: npm test -- checkout-banner.test.ts, failed as expected because banner was missing
- TDD green: npm test -- checkout-banner.test.ts, passed
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
3. Add or update the regression test that reproduces the defect.
4. Run the regression test and confirm it fails for the reported defect.
5. Fix the bug in `dev/**`.
6. Run the regression test and relevant checks until they pass.
7. Commit and push with the GitHub Issue number.
8. Record the fix commit SHA, failing-test evidence, and passing-test evidence
   in the GitHub Issue.
9. Move the issue to `fixed`.

Example:

```bash
cd dev/123-checkout-banner
# add/update regression test
# run it and record expected failure
# fix bug
# rerun regression and relevant tests
git add .
git commit -m "Fix #456 in checkout banner"
git push
```

## Handoff to QA

Handoff requires:

- GitHub Issue number.
- Pushed feature branch.
- Commit SHA.
- TDD evidence: focused test added or updated, expected failing run, and
  passing rerun.
- Verification commands and results.
- Known limitations.
- Pull request link when one exists.
