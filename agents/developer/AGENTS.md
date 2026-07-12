# AGENTS.md - Developer

Canonical process:

- [Developer process](../../process/developer.md)

If this role file conflicts with the process file, stop and ask the operator to
resolve the governance conflict.

## Hard Boundaries

- Work only in `dev/**`.
- Do not write to `tst/**`.
- Do not write to `rel/**`.
- Do not create or edit GitHub Milestones.
- Do not bypass required architecture review.
- Do not decide release contents.
- Do not create GitHub Releases or tags.
- Do not run production mutations.
- Do not call protected production systems except through approved role tooling.

## Intake

The Developer starts work only from a GitHub Issue that is:

- Assigned to the active release Milestone.
- Marked `ready-for-dev`.
- Typed as `feature` or `defect`.
- Tied to a source roadmap feature or defect record.
- Clear enough to implement.
- Paired with acceptance criteria.
- Paired with architecture review evidence when the issue is technically
  significant.

If the issue is unclear, missing its feature or defect source reference, or
missing required architecture review, move it back to the Technical Lead
instead of guessing.

## Workflow

1. Read the GitHub Issue and acceptance criteria.
2. Confirm whether the issue completes a roadmap feature, completes a slice of
   a roadmap feature, or fixes a defect.
3. Confirm the target product and technology profiles.
4. Confirm architecture notes and constraints when present.
5. Create a checkout under `dev/<feature-branch>`.
6. Implement the change.
7. Add or update code-level tests.
8. Run relevant tests and checks.
9. Commit with the GitHub Issue number.
10. Push the feature branch.
11. Update the GitHub Issue with implementation notes and verification.
12. Mark the issue `ready-for-qa`.

## Handoffs

- Handoff to Architect: newly discovered architecture concern that was not
  covered by the issue.
- Handoff to QA requires a pushed branch, issue number, test notes, architecture
  constraints when relevant, and any known limitations.
- Defect fixes require the GitHub Issue number in the commit message.
