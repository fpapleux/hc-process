# Process Overview

This document defines the overall development, test, and release process for a
product folder. It is written for humans and agents.

The goal is to make the filesystem layout match the operating process. Agents
are constrained by folder-level permissions so that product ownership,
architecture, development, QA, and release work happen in separate lanes.

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

Release agents:

- `dev/**`: no access.
- `tst/**`: read access.
- `prod/**`: read/write access.

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
- Product Owner agents turn briefs into GitHub Issues, acceptance criteria, and
  release scope.
- Developer agents do not need access to `prod/main`.
- QA agents do not need to create test checkouts from `prod/main`.
- The Product Owner creates the active GitHub Milestone before feature work
  starts.
- Each feature or defect has a GitHub Issue assigned to the active release
  Milestone before development starts.
- The project works against one active planned release at a time unless the
  Product Owner records an explicit exception.
- Developer agents implement only GitHub Issues marked `ready-for-dev`.
- Feature branches must be committed and pushed before QA tests them.
- QA tests the pushed branch, not uncommitted local developer files.
- Release promotes only tested and approved commits.
- GitHub Milestones track planned releases. GitHub Releases and tags record
  completed production releases.
- Product environment targeting follows [Product Environment Process](environments.md).
- Architecture work follows the [Architecture Design Document Specification](documents/architecture-design-document.md).
- Technically significant issues receive architecture review before
  development starts.
- Durable architecture decisions live in `architecture/**` or approved product
  architecture documentation.

## Lifecycle

The normal lifecycle is:

1. Product Owner turns a brief into GitHub Issues.
2. Product Owner creates or selects the active GitHub Milestone.
3. Product Owner marks technically significant issues `needs-architecture`.
4. Architect records technical direction and marks reviewed issues
   `architecture-reviewed`.
5. Product Owner marks actionable issues `ready-for-dev`.
6. Developer implements an issue in `dev/**`.
7. Developer pushes the feature branch and marks the issue `ready-for-qa`.
8. QA creates a checkout in `tst/**` and validates the pushed branch.
9. QA records evidence and marks passing issues `qa-passed`.
10. Release assembles QA-passed issues into `release/<milestone-name>`.
11. QA validates the assembled release branch.
12. Release promotes the validated release branch to production.
13. Release creates the GitHub Release and tag.

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
- `done`: issue completed and included in a released version.

Defect-specific statuses:

- `open`: QA recorded the failure.
- `accepted`: developer picked up the defect.
- `fixed`: developer pushed a proposed fix.
- `verified`: QA validated the fix.
- `closed`: QA closed the defect after validation.

## Role Process Files

- [Product Owner process](product-owner.md)
- [Architect process](architect.md)
- [Developer process](developer.md)
- [QA process](qa.md)
- [Release process](release.md)
- [Product environment process](environments.md)
- [Architecture Design Document Specification](documents/architecture-design-document.md)
