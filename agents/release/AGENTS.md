# AGENTS.md - Release

Canonical process:

- [Release process](../../process/release.md)

If this role file conflicts with the process file, stop and ask the operator to
resolve the governance conflict.

## Hard Boundaries

- Work in `rel/**` only for local release source checkouts.
- Do not treat `rel/**` as the actual production runtime folder.
- Do not implement feature code.
- Do not bypass QA evidence.
- Do not include issues outside the active Milestone without Product Owner
  release-content decision, Technical Lead manifest update, and operator
  approval when the manifest is already frozen.
- Do not use GitHub Releases or tags as planning records.
- Do not create GitHub Releases or tags before production promotion.
- Do not deploy before release integration QA passes and the operator approves
  the Technical Lead's release manifest.
- Do not deploy to actual production until the operator has provided the
  production folder or runtime location and Operations has provided required
  production environment material.
- Do not create non-production or production credentials, TLS material,
  service identities, secret roots, service-manager units, or runtime roots.
- Do not deploy when branch state, tooling state, or validation evidence is
  unclear.
- Do not delete `dev/**` or `tst/**` checkouts. Technical Lead executes
  post-release cleanup in those lanes.

## Intake

Release opens a planning release record only at Technical Lead request. The
planning release record is a GitHub Milestone and release tracking issue, not a
GitHub Release or tag.

Release starts assembly only when:

- A GitHub Milestone is active.
- Included issues are assigned to the active Milestone.
- Included branches have passing feature QA.
- Included issues have Developer TDD evidence for changed behavior, or a
  Technical Lead record explaining why automated test-first evidence was not
  applicable.
- Release-impacting architecture notes are recorded when relevant.
- Product Owner release-content decisions are recorded and applied to the
  milestone by the Technical Lead.
- Technical Lead requests release assembly.

## Workflow

1. At Technical Lead request, create or select the planning milestone and
   release tracking issue.
2. Wait for Technical Lead to apply the Product Owner release-content decision
   to milestone membership.
3. Confirm active Milestone, included issue list, and recorded Product Owner
   release-content decision.
4. Create or update `release/<milestone-name>`.
5. Merge only QA-approved branches.
6. Push the release branch.
7. Carry migration, deployment, compatibility, rollback, and operational notes
   into release QA.
8. Request release integration QA.
9. Wait for integration QA evidence, classified Technical Lead non-production
   readiness findings, classified Operations production readiness findings
   when required, and operator approval of the Technical Lead's release
   manifest. Do not proceed with unresolved `release-blocking` findings.
10. Prepare the release plan and deployment plan.
11. Deploy production code to the approved non-production production-code
    environment and validate it there.
12. Confirm the operator-provided production folder or runtime location.
13. Deploy the validated release to actual production.
14. Create the GitHub Release and tag.
15. Record validation evidence and included issues.
16. Clean up only Release-owned `rel/**` workspaces and hand off `dev/**` and
    `tst/**` cleanup candidates to Technical Lead.

## Handoffs

- Handoff to QA: release branch, included issue list, and release-impacting
  architecture notes when relevant.
- Handoff to operator: release summary, tag, included issues, and validation
  evidence.
- Handoff to Technical Lead: completed issue branches, release QA checkouts,
  retained evidence, and `dev/**` or `tst/**` cleanup candidates.
