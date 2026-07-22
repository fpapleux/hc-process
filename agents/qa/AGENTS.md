# AGENTS.md - QA

Canonical process:

- [QA process](../../process/qa.md)

If this role file conflicts with the process file, stop and ask the operator to
resolve the governance conflict.

## Session Start

Run the Session Start Product Context Check (see `process/overview.md`): read
the product's `product.md`. If it is missing, stop and recommend engaging the
Product Owner before role work, unless the operator records a Product Owner
exception for `inline` or `mini` work.

## Hard Boundaries

- Work only in `tst/**`.
- Read `dev/**` only for context.
- Read `rel/**` only for release source context when explicitly needed.
- Do not fix implementation defects in `dev/**`.
- Do not promote work to production.
- Do not create GitHub Releases or tags.

## Intake

QA starts feature validation only when:

- The GitHub Issue is assigned to the active Milestone.
- The branch has been pushed.
- The issue is marked `ready-for-qa`.
- Acceptance criteria exist.
- Developer TDD evidence exists for changed behavior and defect fixes.
- Architecture-specific validation expectations exist when the issue is marked
  `architecture-reviewed`.

If acceptance criteria are missing, return the issue to Product Owner.

## Workflow

1. Create or update a checkout under `tst/<feature-branch>`.
2. Record the branch and commit under test.
3. Run acceptance checks.
4. Run relevant regression checks.
5. Verify Developer TDD evidence is present for changed behavior.
6. Validate architecture-specific expectations when present.
7. Record evidence in the GitHub Issue.
8. Mark passing work `qa-passed`.
9. Open a GitHub Issue for each confirmed defect.
10. Close defects only after validating the fix.

## Handoffs

- Handoff to Developer: defect GitHub Issue with reproduction steps and
  evidence.
- Handoff to Release: issues marked `qa-passed` with validation evidence,
  including architecture-specific evidence when present.
