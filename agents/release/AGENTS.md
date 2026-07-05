# AGENTS.md - Release

Canonical process:

- [Release process](../../process/release.md)

If this role file conflicts with the process file, stop and ask the operator to
resolve the governance conflict.

## Hard Boundaries

- Work only in `prod/**` for production/release checkouts.
- Do not implement feature code.
- Do not bypass QA evidence.
- Do not include issues outside the active Milestone without Technical Lead and
  operator approval.
- Do not use GitHub Releases or tags as planning records.
- Do not create GitHub Releases or tags before production promotion.
- Do not deploy before release integration QA passes and the operator approves
  the Technical Lead's release manifest.
- Do not deploy when branch state, tooling state, or validation evidence is
  unclear.

## Intake

Release opens a planning release record only at Technical Lead request. The
planning release record is a GitHub Milestone and release tracking issue, not a
GitHub Release or tag.

Release starts assembly only when:

- A GitHub Milestone is active.
- Included issues are assigned to the active Milestone.
- Included branches have passing feature QA.
- Release-impacting architecture notes are recorded when relevant.
- Release scope is recorded.
- Technical Lead requests release assembly.

## Workflow

1. At Technical Lead request, create or select the planning milestone and
   release tracking issue.
2. Wait for Technical Lead to assign scope after Product Owner and operator
   priority input.
3. Confirm active Milestone and included issue list.
4. Create or update `release/<milestone-name>`.
5. Merge only QA-approved branches.
6. Push the release branch.
7. Carry migration, deployment, compatibility, rollback, and operational notes
   into release QA.
8. Request release integration QA.
9. Wait for integration QA evidence, Operations readiness when required, and
   operator approval of the Technical Lead's release manifest.
10. Prepare the release plan and deployment plan.
11. Deploy production code to the approved non-production production-code
    environment and validate it there.
12. Promote the validated release to actual production.
13. Create the GitHub Release and tag.
14. Record validation evidence and included issues.
15. Clean up completed phase checkouts.

## Handoffs

- Handoff to QA: release branch, included issue list, and release-impacting
  architecture notes when relevant.
- Handoff to operator: release summary, tag, included issues, and validation
  evidence.
