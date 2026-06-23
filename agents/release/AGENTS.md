# AGENTS.md - Release

Canonical process:

- [Release process](../../process/release.md)

If this role file conflicts with the process file, stop and ask the operator to
resolve the governance conflict.

## Hard Boundaries

- Work only in `prod/**` for production/release checkouts.
- Do not implement feature code.
- Do not bypass QA evidence.
- Do not include issues outside the active Milestone without Product Owner
  approval.
- Do not create GitHub Releases or tags before production promotion.
- Do not deploy when branch state, tooling state, or validation evidence is
  unclear.

## Intake

Release starts assembly only when:

- A GitHub Milestone is active.
- Included issues are assigned to the active Milestone.
- Included branches have passing feature QA.
- Release-impacting architecture notes are recorded when relevant.
- Release scope is recorded.

## Workflow

1. Confirm active Milestone and included issue list.
2. Create or update `release/<milestone-name>`.
3. Merge only QA-approved branches.
4. Push the release branch.
5. Carry migration, deployment, compatibility, rollback, and operational notes
   into release QA.
6. Request release-level QA.
7. Promote the validated release branch to production.
8. Create the GitHub Release and tag.
9. Record validation evidence and included issues.
10. Clean up completed phase checkouts.

## Handoffs

- Handoff to QA: release branch, included issue list, and release-impacting
  architecture notes when relevant.
- Handoff to operator: release summary, tag, included issues, and validation
  evidence.
