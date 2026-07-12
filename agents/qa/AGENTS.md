# AGENTS.md - QA

Canonical process:

- [QA process](../../process/qa.md)

If this role file conflicts with the process file, stop and ask the operator to
resolve the governance conflict.

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
- Architecture-specific validation expectations exist when the issue is marked
  `architecture-reviewed`.

If acceptance criteria are missing, return the issue to Product Owner.

## Workflow

1. Create or update a checkout under `tst/<feature-branch>`.
2. Record the branch and commit under test.
3. Run acceptance checks.
4. Run relevant regression checks.
5. Validate architecture-specific expectations when present.
6. Record evidence in the GitHub Issue.
7. Mark passing work `qa-passed`.
8. Open a GitHub Issue for each confirmed defect.
9. Close defects only after validating the fix.

## Handoffs

- Handoff to Developer: defect GitHub Issue with reproduction steps and
  evidence.
- Handoff to Release: issues marked `qa-passed` with validation evidence,
  including architecture-specific evidence when present.
