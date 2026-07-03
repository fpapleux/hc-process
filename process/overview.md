# Process Overview

This document defines the overall development, test, and release process for a
product folder. It is written for humans and agents.

The goal is to make the filesystem layout match the operating process. Agents
are constrained by folder-level permissions so that product ownership, UX
design, architecture, technical leadership, development, QA, and release work
happen in separate lanes.

## Folder Structure

The product folder is a container.

Inside the product folder:

- `dev/`: active development checkouts, one folder per feature branch.
- `tst/`: QA/test checkouts, one folder per feature branch or release branch
  under test.
- `prod/`: production baseline, release checkout, or urgent hotfix checkout.
- `architecture/`: durable architecture decisions, ADRs, and system notes.
- `agents/`: agent role profiles and profile composition rules.
- `process/`: operating process definitions.
- `README.md`: entry point for this product folder.

Example:

```text
website/
  dev/
    123-checkout-banner/
    124-mobile-nav/
  tst/
    123-checkout-banner/
    release-2026.06.1/
  prod/
    main/
  architecture/
  agents/
  process/
  README.md
```

Each feature folder is a Git checkout of the project repository. Do not copy
repositories manually between folders.

## Folder-Level Access Restrictions

Product Owner agents:

- Product folder: read access.
- `dev/**`: no write access.
- `tst/**`: no write access.
- `prod/**`: no write access.
- GitHub Issues: create and edit access.
- GitHub Milestones: create and edit access.
- GitHub repository settings: request access only, unless explicitly delegated.

UX Design Lead agents:

- Product folder: read access.
- `architecture/**`: read access only.
- `dev/**`: no access.
- `tst/**`: no access.
- `prod/**`: no access.
- GitHub Issues: read and comment access.
- GitHub Pull Requests: no access.

Architect agents:

- Product folder: read access.
- `architecture/**`: read/write access.
- Approved product architecture documentation: write access.
- `dev/**`: read access only.
- `tst/**`: read access only when needed for architecture review.
- `prod/**`: read access only when needed for baseline or release-impact
  review.
- GitHub Issues: read, comment, and architecture status access.
- GitHub Pull Requests: read and comment access for architecture review.
- GitHub Milestones: no edit access.

Developer agents:

- `dev/**`: read/write access.
- `tst/**`: no access.
- `prod/**`: no access.

QA agents:

- `dev/**`: read access.
- `tst/**`: read/write access.
- `prod/**`: read access, only to check whether a bug already exists in
  production.

Technical Lead agents:

- Product folder: read access.
- `dev/**`: read/write access (environment setup and maintenance).
- `tst/**`: read/write access (environment setup and maintenance).
- `prod/**`: read access only (baseline reference).
- `architecture/**`: read access only.
- GitHub Issues: full access (create, edit, assign, label, close).
- GitHub Milestones: create and edit access for maintenance milestones.
- GitHub Pull Requests: read access.

Release agents:

- `dev/**`: no access.
- `tst/**`: read access.
- `prod/**`: read/write access.

Operations Lead agents:

- Product folder: read access.
- `dev/**`: no access.
- `tst/**`: no access.
- `prod/**`: read access (monitoring, not promotion or ad-hoc mutation).
- `architecture/**`: read access for architecture documents. Read/write access
  for operations plan documents.
- GitHub Issues: create and edit access for operational issues.
- GitHub Milestones: no edit access.
- GitHub Pull Requests: read access.
- Monitoring and observability systems: full access.
- Production infrastructure: read access for monitoring. Write access only
  through documented runbook procedures.

Promotion between phases is an explicit operation performed by the agent
responsible for the next phase. Developer agents do not promote their own work
into `tst/` or `prod/`.

## Core Rules

- The remote Git branch is the source of truth.
- A phase folder is only a local checkout of that branch for a specific purpose.
- Development happens in `dev/<feature-branch>`.
- QA testing happens in `tst/<feature-branch>`.
- Release testing happens in `tst/release-<milestone-name>`.
- Production and urgent patch work happen in `prod/<branch-or-release>`.
- Product Owner agents define what to build and why. They do not define how to
  build it or make technology choices.
- Product Owner agents turn briefs into GitHub Issues, acceptance criteria, and
  release scope.
- The UX Design Lead translates the product brief into structural UX: user
  flows, wireframes, interaction specs, and accessibility requirements. The
  operator refines the UX with visual design before it reaches the Architect.
- The Technical Lead receives the accepted product brief, UX design document,
  and architecture design document, produces a development plan, and manages
  the GitHub Issues log.
- The Operations Lead activates after the first production release, adapts the
  Architect's initial operations plan to the live environment, and owns
  production system health ongoing.
- Developer agents do not need access to `prod/main`.
- QA agents do not need to create test checkouts from `prod/main`.
- The Product Owner creates the active GitHub Milestone before feature work
  starts. The Technical Lead creates maintenance milestones.
- Each feature or defect has a GitHub Issue assigned to the active release
  Milestone before development starts.
- The project works against one active planned release at a time unless the
  Product Owner records an explicit exception. A maintenance milestone may
  run in parallel.
- Developer agents implement only GitHub Issues marked `ready-for-dev`.
- Feature branches must be committed and pushed before QA tests them.
- QA tests the pushed branch, not uncommitted local developer files.
- Release promotes only tested and approved commits.
- GitHub Milestones track planned releases. GitHub Releases and tags record
  completed production releases.
- Product environment targeting follows [Product Environment Process](environments.md).
- UX design work follows the [UX Design Document Specification](../documents/ux-design-document.md).
- Architecture work follows the [Architecture Design Document Specification](../documents/architecture-design-document.md).
- Technically significant issues receive architecture review before
  development starts.
- Durable architecture decisions, UX design documents, and operations plans
  live in `architecture/**` or approved product documentation.
- Operations plans follow the [Operations Plan Document Specification](../documents/operations-plan.md).

## Feature Lifecycle

The normal feature lifecycle is:

1. Product Owner produces a product brief as GitHub Issues with user goals,
   business outcomes, scope, and acceptance criteria. The brief does not
   prescribe technology or implementation.
2. Product Owner creates or selects the active GitHub Milestone.
3. Operator approves the product brief.
4. UX Design Lead produces the UX design document (markdown and HTML versions):
   user flows, wireframes, interaction specs, accessibility requirements, and
   the design-to-architecture bridge.
5. Operator approves the UX design, applies visual design refinement, and
   hands the approved designs to the Architect.
6. Architect reads the approved UX designs and the product brief. Produces
   the architecture design document (markdown and HTML versions). Marks
   reviewed issues `architecture-reviewed`.
7. Operator approves the architecture design.
8. Technical Lead receives the product brief, UX design document, and
   architecture design document. Produces a development plan: decomposes
   work into GitHub Issues, sequences by dependency, assigns to the
   milestone, prepares environments.
9. Operator approves the development plan.
10. Technical Lead marks actionable issues `ready-for-dev`.
11. Developer implements an issue in `dev/**`.
12. Developer pushes the feature branch and marks the issue `ready-for-qa`.
13. QA creates a checkout in `tst/**` and validates the pushed branch.
14. QA records evidence and marks passing issues `qa-passed`.
15. Release assembles QA-passed issues into `release/<milestone-name>`.
16. QA validates the assembled release branch.
17. Release promotes the validated release branch to production.
18. Release creates the GitHub Release and tag.
19. Operations Lead adapts the operations plan (first release) or reviews the
    operational readiness checklist (subsequent releases), establishes or
    updates monitoring, and begins production oversight.

## Maintenance Lifecycle

Maintenance releases handle defects, dependency updates, operational
improvements, and technical debt. They run on a separate milestone and do not
require a product brief unless the work changes user-visible behavior.

1. QA creates defect issues, Technical Lead identifies maintenance needs, or
   Operations Lead identifies operational issues (`ops-identified`).
2. Technical Lead triages and prioritizes.
3. Technical Lead routes architecturally significant items to the Architect.
4. Architect reviews and records architecture notes when required.
5. Technical Lead creates a maintenance milestone and assigns issues.
6. Technical Lead marks triaged issues `ready-for-dev`.
7. Normal developer → QA → release cycle follows (steps 11-18 above).

## Issue Status Values

- `draft`: issue exists but is not ready for implementation.
- `needs-clarification`: Product Owner needs input before scoping is complete.
- `needs-architecture`: Architect review is required before development.
- `architecture-reviewed`: Architect recorded technical direction and
  implementation constraints.
- `ready-for-dev`: developer agents can pick up the issue.
- `in-dev`: developer agent is implementing the issue.
- `ready-for-qa`: developer pushed the branch and requests QA.
- `qa-passed`: QA accepted the branch.
- `deferred`: issue moved out of the active release.
- `ops-identified`: Operations Lead identified an operational issue in
  production.
- `done`: issue completed and included in a released version.

Defect-specific statuses:

- `open`: QA recorded the failure.
- `accepted`: developer picked up the defect.
- `fixed`: developer pushed a proposed fix.
- `verified`: QA validated the fix.
- `closed`: QA closed the defect after validation.

## Role Process Files

- [Product Owner](product-owner.md)
- [UX Design Lead](ux-design-lead.md)
- [Architect](architect.md)
- [Technical Lead](tech-lead.md)
- [Developer](developer.md)
- [QA](qa.md)
- [Release](release.md)
- [Operations Lead](operations-lead.md)
- [Product environment process](environments.md)
- [UX Design Document Specification](../documents/ux-design-document.md)
- [Architecture Design Document Specification](../documents/architecture-design-document.md)
- [Operations Plan Document Specification](../documents/operations-plan.md)
