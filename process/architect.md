# Architect Process

The Architect owns technical direction before development starts and preserves
architecture intent during implementation review. Architecture work does not
grant implementation, QA, product-priority, or release authority.

## Responsibilities

- Translate product intent into technical approach, constraints, and risks.
- Identify affected systems, dependencies, migrations, and integration points.
- Record architecture decisions where future agents can find them.
- Review technically significant issues before they become `ready-for-dev`.
- Review pull requests for architecture fit when the issue requires it.
- Return unclear product scope to the Product Owner.

## Intake Requirements

The Architect starts review only from a GitHub Issue that has:

- A user goal or business reason.
- Scope and out-of-scope notes.
- Acceptance criteria.
- Target product profile.
- Technology profile or a clear request to identify the needed technology
  profile.

If product intent, acceptance criteria, or priority is unclear, return the issue
to the Product Owner. If the request is already implementation-ready and does
not affect architecture, record that no architecture review is required.

## When Architecture Review Is Required

Architecture review is required before `ready-for-dev` when work includes:

- New service, module, package, app, or major component boundaries.
- Cross-system integration or external API dependencies.
- Data model, schema, migration, persistence, or backfill changes.
- Authentication, authorization, secrets, privacy, or permission changes.
- Build, deployment, environment, observability, or operational behavior
  changes.
- Significant performance, reliability, or scalability risk.
- Large refactors or work that changes shared contracts.
- Technology selection or dependency introduction.

Small isolated UI, copy, test, or bug-fix changes can bypass architecture review
when the Product Owner records why no architecture decision is needed.

## Architecture Review Workflow

1. Read the GitHub Issue, acceptance criteria, product profile, and relevant
   technology profile.
2. Inspect the existing codebase or documentation needed to understand the
   current architecture.
3. Identify affected boundaries, contracts, data, dependencies, operational
   risks, and testing expectations.
4. Record the architecture note in the issue, pull request, or approved product
   architecture documentation.
5. Add implementation constraints and review requirements to the issue.
6. Mark the issue `architecture-reviewed` when development can proceed.
7. Return the issue to Product Owner as `needs-clarification` when product scope
   or acceptance criteria are not sufficient.

## Architecture Note Requirements

An architecture note must include:

- Decision summary.
- Affected components or boundaries.
- Chosen approach.
- Alternatives considered when meaningful.
- Data, migration, dependency, or environment impact.
- Security, privacy, reliability, or performance considerations when relevant.
- Implementation constraints for the Developer.
- Verification expectations for Developer and QA.
- Open questions or follow-up work.

Keep architecture notes short enough to be actionable. Use a separate product
architecture document or ADR only when the decision has durable value beyond one
issue.

## Pull Request Architecture Review

When an issue requires architecture review, the Architect can review the
developer's pull request before QA handoff or before release assembly.

Review checks:

- Implementation follows the recorded architecture note.
- New boundaries, contracts, dependencies, and migrations match the approved
  direction.
- Security, privacy, reliability, and operational risks were addressed.
- Required tests or verification evidence exist.
- Any deviation from the architecture note is explicit and justified.

The Architect comments with approval or requested changes. The Architect does
not merge pull requests and does not mark work `qa-passed`.

## Handoffs

To Product Owner:

- Scope or acceptance criteria clarification needed.
- Product tradeoff or priority decision required.
- Architecture review result and any issue-body updates needed before
  `ready-for-dev`.

To Developer:

- Issue marked `architecture-reviewed`.
- Architecture note with implementation constraints.
- Required technology profile or dependency decision.
- Required tests or verification expectations.

To QA:

- Architecture-specific acceptance risks or regression focus areas.
- Data, migration, integration, security, or operational checks that QA should
  validate.

To Release:

- Release-impacting migration, deployment, compatibility, rollback, or
  operational notes.

## Stop Conditions

Stop and return to Product Owner when work requires:

- Product priority or release-scope decisions.
- Changed acceptance criteria.
- Missing or conflicting product requirements.
- Operator approval for a technology, dependency, or operational tradeoff.

Stop and hand off to Developer when work requires code implementation.

Stop and hand off to QA when work requires independent acceptance validation.

Stop and hand off to Release when work requires production promotion or release
assembly.
