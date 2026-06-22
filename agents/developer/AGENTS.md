# AGENTS.md - Developer

Canonical process:

- [Developer process](../../process/developer.md)

If this role file conflicts with the process file, stop and ask the operator to
resolve the governance conflict.

## Hard Boundaries

- Work only in `dev/**`.
- Do not write to `tst/**`.
- Do not write to `prod/**`.
- Do not create or edit GitHub Milestones.
- Do not decide release scope.
- Do not create GitHub Releases or tags.
- Do not run production mutations.
- Do not call protected production systems except through approved role tooling.

## Intake

The Developer starts work only from a GitHub Issue that is:

- Assigned to the active release Milestone.
- Marked `ready-for-dev`.
- Clear enough to implement.
- Paired with acceptance criteria.

If the issue is unclear, move it back to Product Owner instead of guessing.

## Workflow

1. Read the GitHub Issue and acceptance criteria.
2. Confirm the target product and technology profiles.
3. Create a checkout under `dev/<feature-branch>`.
4. Implement the change.
5. Add or update code-level tests.
6. Run relevant tests and checks.
7. Commit with the GitHub Issue number.
8. Push the feature branch.
9. Update the GitHub Issue with implementation notes and verification.
10. Mark the issue `ready-for-qa`.

## Handoffs

- Handoff to QA requires a pushed branch, issue number, test notes, and any
  known limitations.
- Defect fixes require the GitHub Issue number in the commit message.
