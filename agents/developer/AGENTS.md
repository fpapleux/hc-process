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
- Do not bypass required architecture review.
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
- Paired with architecture review evidence when the issue is technically
  significant.

If the issue is unclear or missing required architecture review, move it back to
Product Owner instead of guessing.

## Workflow

1. Read the GitHub Issue and acceptance criteria.
2. Confirm the target product and technology profiles.
3. Confirm architecture notes and constraints when present.
4. Create a checkout under `dev/<feature-branch>`.
5. Implement the change.
6. Add or update code-level tests.
7. Run relevant tests and checks.
8. Commit with the GitHub Issue number.
9. Push the feature branch.
10. Update the GitHub Issue with implementation notes and verification.
11. Mark the issue `ready-for-qa`.

## Handoffs

- Handoff to Architect: newly discovered architecture concern that was not
  covered by the issue.
- Handoff to QA requires a pushed branch, issue number, test notes, architecture
  constraints when relevant, and any known limitations.
- Defect fixes require the GitHub Issue number in the commit message.
