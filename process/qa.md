# QA Process

QA validates pushed branches in `tst/**`. QA records acceptance evidence,
creates GitHub Issues for defects, and closes defects only after validation
passes.

## Intake Requirements

QA starts feature validation only when:

- The GitHub Issue is assigned to the active Milestone.
- The branch has been pushed.
- The issue is marked `ready-for-qa`.
- Acceptance criteria exist.
- CI evidence exists for the exact pushed branch and commit when the repository
  has CI, unless the Technical Lead explicitly recorded a waiver.
- Architecture-specific validation expectations are recorded when the issue is
  marked `architecture-reviewed`.

If acceptance criteria are missing, return the issue to Product Owner.

## Preparing a Feature for Test

QA creates its own test checkout from the pushed branch:

```bash
git clone git@github.com:OWNER/REPO.git tst/123-checkout-banner
cd tst/123-checkout-banner
git switch --track origin/123-checkout-banner
```

This keeps QA independent from uncommitted developer state. The `tst/**`
checkout is a test checkout of the branch, not the source of truth.

## Feature Validation

The QA agent works in `tst/<feature-branch>`:

```bash
cd tst/123-checkout-banner
git status
# run acceptance checks, smoke tests, browser tests, or manual verification
```

QA owns acceptance evidence:

- Test command output.
- Reproduction steps.
- Screenshots or logs when useful.
- Exact branch and commit tested.
- CI workflow/run identifier, commit SHA, and pass/fail status when CI exists.
- Clear pass/fail result.
- Architecture-specific validation evidence when requested by the Architect.

If the repository has CI and no CI run exists for the tested branch/commit, QA
does not mark the issue `qa-passed` unless the Technical Lead explicitly
waived CI for that issue. QA records the missing run as a blocker or defect
with the branch, commit, expected workflow, and observed workflow state.

QA writes higher-level checks when the project requires them, such as browser
flows, smoke tests, fixture validations, or deployment checks. The Developer
remains responsible for turning confirmed bugs into code-level regression tests.

## Release Integration Validation

After Release assembles `release/<milestone-name>`, QA validates the assembled
release branch in `tst/release-<milestone-name>`. This validation is separate
from feature-level QA. Passing individual issues is necessary, but it is not
sufficient for release approval.

Release integration QA verifies:

- The release branch contains only issues assigned to the milestone and not
  explicitly deferred.
- Merged behavior works across issue boundaries.
- Regression, smoke, browser, CLI, API, migration, or deployment rehearsal
  checks required by the product pass against the assembled release branch.
- Required CI passes for the release branch commit when CI exists, unless the
  Technical Lead explicitly records a waiver.
- Release-impacting architecture notes and operational validation expectations
  are addressed.
- No release evidence depends on bypassed validation, fixture credentials,
  dummy TLS material, disabled certificate verification, or browser security
  exceptions.

QA records release integration evidence in the release tracking issue or
milestone before handing the release back to the Technical Lead and Release.

## Defect Management

QA does not move the codebase back to `dev/`.

Every confirmed QA failure gets a GitHub Issue before corrective development
work starts.

The defect record contains:

- GitHub Issue number.
- Feature branch.
- Failing commit SHA.
- Test command or acceptance check that failed.
- Expected behavior.
- Actual behavior.
- Reproduction steps.
- Logs, screenshots, or artifacts when relevant.
- Current owner.
- Current status.

Defect statuses:

- `open`: QA recorded the failure.
- `accepted`: developer picked up the defect.
- `fixed`: developer pushed a proposed fix.
- `verified`: QA validated the fix.
- `closed`: QA closed the defect after validation.

Required loop:

1. QA creates the defect record with failing branch, commit, command, and
   observed behavior.
2. Developer accepts the defect and becomes the owner.
3. Developer fixes the bug in `dev/**`.
4. Developer adds or updates the regression test.
5. Developer commits and pushes with the GitHub Issue number in the commit
   message.
6. Developer records the fix in the GitHub Issue, including the fix commit SHA
   and tests run.
7. QA updates `tst/**` to the new pushed commit and retests.
8. QA records validation evidence in the defect record.
9. QA closes the defect only after the validation passes.

QA retest:

```bash
cd tst/123-checkout-banner
git fetch origin
git reset --hard origin/123-checkout-banner
# rerun QA checks
```

## Handoff to Release

QA marks work `qa-passed` only when:

- Acceptance criteria pass.
- Relevant regression checks pass.
- Required CI passes for the exact tested commit, unless explicitly waived by
  Technical Lead.
- Architecture-specific validation expectations pass when present.
- Defects are closed or explicitly deferred by the Product Owner.
- Evidence is recorded in the GitHub Issue.
