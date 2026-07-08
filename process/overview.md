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

## New Product Repository Bootstrap

When a product is new and no target repository exists yet, bootstrap the
repository before development planning and issue decomposition.

The Technical Lead performs this bootstrap after the Product Owner brief, UX
design document, and architecture design document are approved:

1. Identify the target production folder, for example `/opt/api`.
2. Go to the target production folder.
3. If the target production folder does not exist, stop and ask the operator to
   create it. Agents must not create the target production folder themselves.
4. Create the new GitHub repository for the product.
5. Initialize a new Git repository in the target production folder.
6. Set the GitHub repository as the remote.
7. Create `README.md`.
8. Commit and push `README.md` to `main`.
9. Return to the product process folder.
10. Create the phase folders `dev/`, `tst/`, and `prod/`.

After this bootstrap, the product process folder is ready for development
planning: GitHub Milestones and Issues can be created against the new
repository, and development checkouts can be created under `dev/**`.

## Folder-Level Access Restrictions

Product Owner agents:

- Product folder: read access.
- `dev/**`: no write access.
- `tst/**`: no write access.
- `prod/**`: no write access.
- GitHub Issues: create and edit access.
- GitHub Milestones: read access and priority-intent recording; create or edit
  only when explicitly delegated by Technical Lead or operator.
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
- GitHub repository creation: allowed only for new product bootstrap after the
  operator-approved brief, UX, and architecture exist.
- GitHub Issues: full access (create, edit, assign, label, close).
- GitHub Milestones: create and edit access for maintenance milestones; assign
  issues to the active planned release milestone after Product Owner and
  operator priority input; coordinate planned release milestone creation with
  Release.
- GitHub Pull Requests: read access.

Release agents:

- `dev/**`: no access.
- `tst/**`: read access.
- `prod/**`: read/write access.
- GitHub Issues: create release tracking issues at Technical Lead request;
  comment with release, promotion, and smoke evidence.
- GitHub Milestones: create or select one active planning milestone at
  Technical Lead request; do not assign issues unless Technical Lead explicitly
  requests assignment.
- GitHub Releases: create releases and tags only after production promotion.

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
- Non-production and production infrastructure: read access for monitoring.
  Write access only through documented environment or runbook procedures.
- Host-managed environment material: create, rotate, permission, and maintain
  scoped environment secrets, TLS material, service-manager configuration, and
  runtime directories outside Git.

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
- Product briefs include a roadmap feature table with `V1 features`,
  `V2 features`, and `Beyond features`; roadmap feature names become source
  references for later GitHub Issues.
- The UX Design Lead translates the product brief into structural UX: user
  flows, wireframes, interaction specs, and accessibility requirements. The
  operator refines the UX with visual design before it reaches the Architect.
- The Technical Lead receives the accepted product brief, UX design document,
  and architecture design document, produces a development plan, and manages
  the GitHub Issues log.
- For a new product with no repository yet, the Technical Lead bootstraps the
  target production folder repository first, then creates `dev/`, `tst/`, and
  `prod/` in the product process folder before encoding the development plan in
  GitHub Issues.
- The Operations Lead activates after the first production release, adapts the
  Architect's initial operations plan to the live environment, and owns
  production system health ongoing.
- The Operations Lead may also be invoked before first production release for
  scoped non-production environment provisioning when Release is blocked on
  approved host-managed credentials, TLS material, service-manager setup, or
  operational runtime ownership.
- Developer agents do not need access to `prod/main`.
- QA agents do not need to create test checkouts from `prod/main`.
- The Product Owner records release priority intent before feature work starts.
  The Release Manager opens the planning release record in GitHub at Technical
  Lead request: a GitHub Milestone and release tracking issue, not a GitHub
  Release or tag. For a new product whose repository does not exist yet, the
  Product Owner records the intended active milestone and the Technical Lead
  coordinates opening it immediately after repository bootstrap. The Technical
  Lead creates maintenance milestones when the release is maintenance-only.
- Each feature or defect has a GitHub Issue assigned to the active release
  Milestone before development starts.
- Every developer-executable issue must directly complete a roadmap feature,
  complete an explicit slice of a roadmap feature, or fix a defect. No issue
  may be marked `ready-for-dev` without a `feature` or `defect` type and a
  source reference to the Product Owner roadmap feature or defect record.
- The project works against one active planned release at a time unless the
  Product Owner records an explicit exception. A maintenance milestone may
  run in parallel.
- Every product, platform, feature, tool, and runtime we build must enforce
  production/non-production runtime separation. A single UI, API, service,
  worker, scheduler, CLI daemon, shared registry, or shared process must not be
  able to select, proxy, switch between, or manage both worlds.
- Developer agents implement only GitHub Issues marked `ready-for-dev`.
- Feature branches must be committed and pushed before QA tests them.
- QA tests the pushed branch, not uncommitted local developer files.
- Release promotes only tested and approved commits.
- GitHub Milestones track planned releases. GitHub Releases and tags record
  completed production releases.
- Release scope is selected by the Technical Lead after Product Owner and
  operator priority input. Scope freezes after operator approval of the release
  manifest.
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

1. Product Owner produces a product brief with user goals, business outcomes,
   scope, acceptance criteria, and a roadmap feature table listing V1, V2, and
   Beyond features. The brief does not prescribe technology or implementation.
2. Product Owner records the intended release priority and milestone intent.
   The GitHub Milestone is opened later by Release at Technical Lead request
   when the repository exists.
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
8. For a new product with no repository yet, Technical Lead performs the new
   product repository bootstrap from the target production folder and creates
   `dev/`, `tst/`, and `prod/` in the product process folder.
9. For a new product whose active milestone was only recorded by the Product
   Owner, Technical Lead asks Release to open the planning release record in
   GitHub after repository bootstrap.
10. Technical Lead receives the product brief, UX design document, and
    architecture design document. Produces a development plan: creates issue
    coverage for the in-scope Product Owner roadmap features, decomposes work
    into GitHub Issues, records the source feature or defect for each developer
    issue, sequences by dependency, assigns to the milestone, and prepares
    environments.
11. Operator approves the development plan.
12. Technical Lead asks Release to open the planning release record in GitHub:
    a GitHub Milestone and release tracking issue. This is not a GitHub Release
    or tag.
13. Product Owner and operator provide release priority input. Technical Lead
    assigns selected issues to the milestone, fills remaining capacity based on
    dependencies and risk, and marks the next actionable issue `ready-for-dev`.
14. Developer implements one issue in `dev/**`.
15. Developer pushes the feature branch, verifies CI started when CI exists,
    records branch/commit/check evidence, and marks the issue `ready-for-qa`.
16. QA creates a checkout in `tst/**` and validates the pushed branch and CI
    result for the exact tested commit when CI exists.
17. QA records evidence and marks passing issues `qa-passed`. Blocking defects
    return to Developer and then QA before release inclusion unless explicitly
    deferred.
18. Technical Lead repeats the Developer -> QA alternation until all selected
    issues are `qa-passed`, fixed, or explicitly deferred.
19. Release assembles QA-passed issues into `release/<milestone-name>`.
20. QA validates the assembled release branch in `tst/release-<milestone-name>`
    and records release integration evidence.
21. Operations Lead validates operational readiness when infrastructure,
    runtime, credentials, TLS, browser trust, monitoring, or deployment
    ownership can affect the release. This may run concurrently with release
    integration QA.
22. Technical Lead produces release notes and a release manifest. Operator
    approval of the manifest freezes release scope.
23. Release prepares the release plan and deployment plan.
24. Release deploys production code first to the approved non-production
    production-code environment and validates deployment there.
25. Release promotes the validated release to actual production.
26. Release creates the GitHub Release and tag.
27. Operations Lead adapts the operations plan (first release) or reviews the
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
7. Normal Technical Lead-orchestrated Developer -> QA -> Release -> integration
   QA -> Operations -> manifest approval -> staged deployment cycle follows.

## Issue Status Values

- `draft`: issue exists but is not ready for implementation.
- `needs-clarification`: Product Owner needs input before scoping is complete.
- `needs-architecture`: Architect review is required before development.
- `architecture-reviewed`: Architect recorded technical direction and
  implementation constraints.
- `ready-for-dev`: developer agents can pick up the issue.
- `in-dev`: developer agent is implementing the issue.
- `ready-for-qa`: developer pushed the branch, recorded verification evidence,
  and recorded CI evidence or a Technical Lead CI waiver when CI exists.
- `qa-passed`: QA accepted the branch and required CI for the tested commit
  passed or was explicitly waived by Technical Lead.
- `deferred`: issue moved out of the active release.
- `ops-identified`: Operations Lead identified an operational issue in
  a managed environment.
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
