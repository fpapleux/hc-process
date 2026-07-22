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
- `rel/`: local release source checkouts. `rel/main` is the local checkout of
  the production baseline (`main`) used for support, approved hotfix source
  work, and non-production runtime use. Other `rel/**` folders hold release
  branches when needed. This is not the actual production runtime folder.
- `documentation/`: the single home for all durable product documentation —
  product briefs, UX design documents, architecture design documents, ADRs and
  system notes, operations plans, and product-specific process documentation.
  Created by the Product Owner on first engagement. Product documentation does
  not live anywhere else.
- `agents/`: agent role profiles and profile composition rules.
- `process/`: operating process definitions.
- `themes/`: reusable visual identity themes for generated web surfaces.
- `product.md`: product context record. The durable link between this product
  and its source repository, brief, theme, active milestone, and lifecycle
  stage. Created and maintained by the Product Owner. Its presence marks that
  the Product Owner has established product context.
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
  rel/
    main/
  documentation/
    website-brief.md
    website-brief.html
    website-ux-design.md
    website-architecture.md
  agents/
  process/
  product.md
  README.md
```

Each feature folder is a Git checkout of the project repository. Do not copy
repositories manually between folders.

## Product Context Record

`product.md` is the product context record at the root of the product folder.
It is the single durable marker that the Product Owner has established product
context, and the canonical link between the process and the product's source
repository. The Product Owner creates it on first engagement and keeps it
current. Other roles read it but do not own it.

The record captures:

- Product name and one-line framing.
- Assurance level and artifact budget.
- Source repository: remote URL and local product-folder path.
- Product brief: location and status (`none`, `draft`, `approved`).
- Selected theme and theme mode, when the product has a visual surface.
- Active milestone or recorded release intent.
- Lifecycle stage: brief, ux, architecture, planning, development, qa, release,
  or operations.
- Date the Product Owner first engaged and the last update.

Template:

```text
# Product Context — <product name>

- Framing: <one line>
- Assurance level: Prototype | Personal Productivity | Production Grade | Enterprise / Regulated
- Artifact budget: inline | mini | standard | full
- Source repository: <remote URL or "not created yet">
- Local product folder: <path>
- Documentation folder: documentation/
- Product brief: <path or "none"> — status: none | draft | approved
- Theme: <theme or "n/a"> — mode: light | dark | both | system
- Active milestone / release intent: <milestone or intent>
- Lifecycle stage: brief | ux | architecture | planning | development | qa | release | operations
- PO first engaged: YYYY-MM-DD
- Last updated: YYYY-MM-DD by <role>
```

For `inline` and `mini` work where the operator intentionally proceeds without a
Product Owner brief, the record instead carries a Product Owner exception block:

```text
## Product Owner Exception (inline/mini only)

- Reason: <why Product Owner product definition is skipped>
- Requested by: <operator approval source>
- Scope limit: <what this exception covers>
- Date: YYYY-MM-DD
```

## Session Start Product Context Check

On the first interaction of a session, before doing role work, the active agent
establishes product context:

1. Identify the target product folder from the operator's request, the current
   workspace, or by asking when it is ambiguous.
2. Read `product.md` at the product folder root.

Then:

- If `product.md` exists, state the resolved product context (product, source
  repository, brief status, theme, active milestone, lifecycle stage) and
  continue in the active role.
- If `product.md` is missing, the Product Owner has not established product
  context:
  - If the active role is Product Owner, establish product context before
    defining the product:
    1. Confirm with the operator that the product will be developed in the
       current folder. This is required before building can start. If it is the
       wrong folder, recommend restarting the session from the correct
       development folder and stop.
    2. Create the `documentation/` folder in the current folder. It holds all
       product and process documentation for the product.
    3. Create `product.md`.
    4. Then begin product definition.
  - If the active role is not Product Owner, stop and recommend engaging the
    Product Owner first. Continue only when the operator engages the Product
    Owner or records a Product Owner exception in `product.md` for `inline` or
    `mini` work.

This check applies to every artifact budget. `inline` and `mini` work may
satisfy it with a Product Owner exception block instead of a full Product Owner
brief.

## New Product Source Repository Bootstrap

When a product is new and no target repository exists yet, bootstrap the
repository before development planning and issue decomposition.

The Technical Lead performs this bootstrap after the Product Owner brief, UX
design document, and architecture design document are approved:

1. Identify or create the GitHub source repository for the product.
2. Create the phase folders `dev/`, `tst/`, and `rel/` in the product process
   folder when they do not exist.
3. Initialize or clone the source repository in `rel/main`.
4. Create `README.md` when the repository is otherwise empty.
5. Commit and push `README.md` to `main`.

After this bootstrap, the product process folder is ready for development
planning: GitHub Milestones and Issues can be created against the new
repository, and development checkouts can be created under `dev/**`.

The actual production folder or runtime location is not created during source
repository bootstrap. The operator provides that location before actual
production deployment, during the Release process.

## Folder-Level Access Restrictions

Product Owner agents:

- Product folder: read access; create the `documentation/` folder and write
  `product.md` at the product folder root.
- `documentation/`: write access for product briefs and product-owned product
  documentation; read access for documents owned by other roles.
- `dev/**`: no write access.
- `tst/**`: no write access.
- `rel/**`: no write access.
- GitHub Issues: create and edit access.
- GitHub Milestones: read access and release-intent recording only. Product
  Owner agents do not create or edit planning Milestones and do not assign,
  remove, or move issues between Milestones.
- GitHub repository settings: request access only, unless explicitly delegated.

UX Design Lead agents:

- Product folder: read access.
- `documentation/`: write access for UX design documents; read access for
  documents owned by other roles.
- `dev/**`: no access.
- `tst/**`: no access.
- `rel/**`: no access.
- GitHub Issues: read and comment access.
- GitHub Pull Requests: no access.

Architect agents:

- Product folder: read access.
- `documentation/`: write access for architecture design documents, ADRs, and
  system notes; read access for documents owned by other roles.
- `dev/**`: read access only.
- `tst/**`: read access only when needed for architecture review.
- `rel/**`: read access only when needed for baseline or release-impact
  review.
- GitHub Issues: read, comment, and architecture status access.
- GitHub Pull Requests: read and comment access for architecture review.
- GitHub Milestones: no edit access.

Developer agents:

- `dev/**`: read/write access.
- `tst/**`: no access.
- `rel/**`: no access.

QA agents:

- `dev/**`: read access.
- `tst/**`: read/write access.
- `rel/**`: read access only for release source or production-baseline context
  when explicitly needed.

Technical Lead agents:

- Product folder: read access.
- `dev/**`: read/write access (environment setup and maintenance).
- `tst/**`: read/write access (environment setup and maintenance).
- `rel/**`: read access for baseline reference; write access for
  non-production runtime setup under Technical Lead authority when required.
- `documentation/`: read access.
- GitHub repository creation: allowed only for new product bootstrap after the
  operator-approved brief, UX, and architecture exist.
- GitHub Issues: full access (create, edit, assign, label, close).
- GitHub Milestones: create and edit access for maintenance milestones; arrange
  missing non-major release/milestone records; coordinate major version release
  records after operator order; assign issues to the active planned release
  milestone according to Product Owner included/deferred issue decisions;
  coordinate planning milestone creation with Release when the process uses a
  Release handoff for the planning record.
- GitHub Pull Requests: read access.
- Non-production infrastructure and environment material: create, rotate,
  permission, and maintain scoped non-production secrets, TLS material,
  service-manager configuration, monitoring, logs, runbooks, and runtime
  directories outside Git.

Release agents:

- `dev/**`: no access.
- `tst/**`: read access.
- `rel/**`: read/write access.
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
- `rel/**`: read access when release source context is needed.
- `documentation/`: write access for operations plan documents; read access for
  documents owned by other roles.
- GitHub Issues: create and edit access for operational issues.
- GitHub Milestones: no edit access.
- GitHub Pull Requests: read access.
- Monitoring and observability systems: full access.
- Production infrastructure: read access for monitoring. Write access only
  through documented environment or runbook procedures.
- Production environment material: create, rotate, permission, and maintain
  scoped production secrets, TLS material, service-manager configuration, and
  runtime directories outside Git.

Promotion between phases is an explicit operation performed by the agent
responsible for the next phase. Developer agents do not promote their own work
into `tst/` or `rel/`.

## Core Rules

- Every session begins with the Session Start Product Context Check. The product
  context record `product.md` is the durable marker that the Product Owner has
  established product context. A non-Product-Owner role stops and recommends the
  Product Owner when `product.md` is missing, unless an operator-recorded
  Product Owner exception covers `inline` or `mini` work.
- On first engagement, the Product Owner confirms with the operator that the
  product will be developed in the current folder before building starts. If it
  is the wrong folder, the recommendation is to restart the session from the
  correct development folder. The Product Owner then creates the `documentation/`
  folder that holds all product and process documentation.
- The remote Git branch is the source of truth.
- A phase folder is only a local checkout of that branch for a specific purpose.
- Development happens in `dev/<feature-branch>`.
- QA testing happens in `tst/<feature-branch>`.
- Release testing happens in `tst/release-<milestone-name>`.
- Release source-control work happens in `rel/<branch-or-release>`. `rel/main`
  is the local checkout of the production baseline (`main`) for support,
  approved hotfix source work, and non-production runtime use. Actual
  production runtime folders are outside the product process folder and are
  supplied by the operator before production deployment.
- Product Owner agents define what to build and why. They do not define how to
  build it or make technology choices.
- Theme controls visual identity for visual web surfaces. It does not grant
  product, technical, QA, or release authority.
- Product Owner agents turn briefs into GitHub Issues, acceptance criteria,
  product priority, and included/deferred issue decisions for releases.
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
  source repository in `rel/main`, then creates `dev/`, `tst/`, and `rel/` in
  the product process folder before encoding the development plan in GitHub
  Issues.
- The Operations Lead activates after the first production release, adapts the
  Architect's initial operations plan to the live environment, and owns
  production system health ongoing.
- The Technical Lead owns non-production environment readiness, including
  maintained non-production runtime material required for release rehearsal and
  smoke validation.
- Developer agents do not need access to `rel/main`.
- QA agents do not need to create test checkouts from `rel/main`.
- The Product Owner records release priority intent and decides which issues
  are included in or deferred from the next release before feature work starts.
  The Release Manager opens the planning release record in GitHub at Technical
  Lead request when the process uses a Release handoff for that record: a
  GitHub Milestone and release tracking issue, not a GitHub Release or tag. For
  a new product whose repository does not exist yet, the Product Owner records
  the intended active milestone and release contents, and the Technical Lead
  coordinates opening the milestone immediately after repository bootstrap. The
  Technical Lead creates maintenance milestones when the release is
  maintenance-only.
- Every visual web surface must have a selected theme before implementation
  starts. For `standard` and `full` work, record the theme in the product brief
  and UX design document. For `inline` and `mini` work, an issue note is enough.
  Use `leantime-inspired` by default until a product records a different theme.
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
- Development is test-driven. For every feature, defect fix, refactor, or
  behavior change, Developer writes or updates the relevant automated test
  first, runs it and records the expected failure, then implements the minimal
  change needed to pass before refactoring.
- For visual behavior, test-first evidence should cover the smallest useful
  observable contract: rendered component state, accessibility role/name,
  keyboard/focus behavior, snapshot, screenshot, or CSS-token assertion,
  depending on the product's technology profile.
- Feature branches must be committed and pushed before QA tests them.
- QA tests the pushed branch, not uncommitted local developer files.
- Release promotes only tested and approved commits.
- GitHub Milestones track planned releases. GitHub Releases and tags record
  completed production releases.
- Release contents are selected by the Product Owner. The Technical Lead
  applies those decisions to milestone membership, validates sequencing and
  dependencies, and prepares the release manifest. Release contents freeze
  after operator approval of the release manifest.
- Product environment targeting follows [Product Environment Process](environments.md).
- UX design work follows the [UX Design Document Specification](../documents/ux-design-document.md).
- Architecture work follows the [Architecture Design Document Specification](../documents/architecture-design-document.md).
- Technically significant issues receive architecture review before
  development starts.
- All product documentation — product briefs, UX design documents, architecture
  design documents, ADRs, and operations plans — lives in the product's
  `documentation/` folder. Product documentation is not scattered across other
  locations.
- Operations plans follow the [Operations Plan Document Specification](../documents/operations-plan.md).

## Approval Evidence

When the process requires operator approval, the next role does not need a
special approval artifact or template. Before proceeding, it must be able to
point to the approval source: a conversation message, GitHub issue comment,
document status update, pull request comment, or other durable record visible
to the operator and downstream roles.

If approval authority has been explicitly delegated, cite that delegated
approval source instead. If the role cannot point to the approval source, it
stops and asks before proceeding.

## Waiver Records

Waivers are durable exception records, not informal permission. A waiver lives
on the GitHub Issue or release tracking issue that owns the blocked gate:

- Feature CI waiver: record on the feature or defect issue.
- QA or retest waiver: record on the feature or defect issue being validated.
- Release readiness or smoke waiver: record on the release tracking issue.
- Production readiness waiver: record on the release tracking issue, with links
  to any Operations issue.

Every waiver record must include:

- Waiver scope: issue, release, commit, environment, and check being waived.
- Reason: why the required evidence cannot be produced now.
- Approver: Technical Lead for CI or QA-process waivers; operator for release,
  smoke, readiness, or production waivers.
- Risk owner: person or role accepting the risk until resolution.
- Expiry: date, release, commit SHA, or environment boundary where the waiver
  stops applying.
- Follow-up issue: required for release, smoke, readiness, and production
  waivers; required for CI waivers unless the issue itself resolves the gap.

Waivers do not silently carry across releases, commits, or environments. A
waiver must be renewed with a new record when its expiry boundary is crossed.

Non-waivable gates:

- Production/non-production runtime separation.
- Secret exposure in commits, issues, pull requests, release notes, logs, or
  chat.
- Production credentials used for non-production checks.
- Dummy credentials, dummy TLS material, disabled certificate verification, or
  browser security exceptions used as release evidence.
- Unresolved readiness findings classified `release-blocking`.

## Feature Lifecycle

Artifact budgets scale this lifecycle. `inline` and `mini` work still records
calibration, acceptance criteria, decisions, and verification, but it may use
issue notes or scoped documents instead of full standalone artifacts. Full PO,
UX, architecture, and operations documents are required only when the calibrated
budget calls for them or the operator asks.

The normal feature lifecycle is:

1. Product Owner runs the Session Start Product Context Check, confirms with the
   operator that the product will be developed in the current folder, creates
   the `documentation/` folder, and creates `product.md`. If it is the wrong
   folder, the Product Owner recommends restarting from the correct development
   folder and stops.
2. Product Owner produces the product brief in markdown and HTML versions with
   user goals, business outcomes, scope, acceptance criteria, and a roadmap
   feature table listing V1, V2, and Beyond features. The markdown version is
   canonical; the HTML version is mandatory and must match the markdown
   content. The brief does not prescribe technology or implementation. The brief
   is saved in the product's `documentation/` folder.
3. Product Owner records the intended release priority and milestone intent.
   The GitHub Milestone is opened later by Release at Technical Lead request
   when the repository exists.
4. Operator approves the product brief.
5. UX Design Lead produces the UX design document (markdown and HTML versions):
   user flows, wireframes, interaction specs, accessibility requirements, and
   the design-to-architecture bridge. The document is saved in the product's
   `documentation/` folder.
6. Operator approves the UX design, applies visual design refinement, and
   hands the approved designs to the Architect.
7. Architect reads the approved UX designs and the product brief. Produces
   the architecture design document (markdown and HTML versions) in the
   product's `documentation/` folder. Marks reviewed issues
   `architecture-reviewed`.
8. Operator approves the architecture design.
9. For a new product with no repository yet, Technical Lead performs the new
   product source repository bootstrap in `rel/main` and creates `dev/`,
   `tst/`, and `rel/` in the product process folder.
10. For a new product whose active milestone was only recorded by the Product
    Owner, Technical Lead asks Release to open the planning release record in
    GitHub after repository bootstrap.
11. Technical Lead receives the product brief, UX design document, and
    architecture design document. Produces a development plan: creates issue
    coverage for the in-scope Product Owner roadmap features, decomposes work
    into GitHub Issues, records the source feature or defect for each developer
    issue, sequences by dependency, assigns to the milestone, and prepares
    environments.
12. Operator approves the development plan.
13. Technical Lead asks Release to open the planning release record in GitHub:
    a GitHub Milestone and release tracking issue. This is not a GitHub Release
    or tag.
14. Product Owner decides which issues are included in or deferred from the
    next release. Technical Lead applies included issues to the milestone,
    validates required documentation, information, dependencies, risk, and
    environment readiness, then marks the next executable issue
    `ready-for-dev`.
15. Developer implements one issue in `dev/**` using TDD: failing test first,
    minimal implementation second, refactor only after the focused test and
    relevant checks pass.
16. Developer pushes the feature branch, verifies CI started when CI exists,
    records branch/commit/check evidence plus TDD evidence, and marks the issue
    `ready-for-qa`.
17. QA creates a checkout in `tst/**` and validates the pushed branch and CI
    result for the exact tested commit when CI exists.
18. QA records evidence and marks passing issues `qa-passed`. Blocking defects
    return to Developer and then QA before release inclusion unless explicitly
    deferred.
19. Technical Lead repeats the Developer -> QA alternation until all selected
    issues are `qa-passed`, fixed, or explicitly deferred.
20. Release assembles QA-passed issues into `release/<milestone-name>`.
21. QA validates the assembled release branch in `tst/release-<milestone-name>`
    and records release integration evidence.
22. Technical Lead validates non-production readiness when infrastructure,
    runtime, credentials, TLS, browser trust, monitoring, or deployment
    ownership can affect release rehearsal. Operations Lead validates
    production readiness when actual production deployment is in scope. These
    checks may run concurrently with release integration QA.
23. Technical Lead produces release notes and a release manifest. Operator
    approval of the manifest freezes release contents.
24. Release prepares the release plan and deployment plan.
25. Release deploys production code first to the approved non-production
    production-code environment and validates deployment there.
26. Release deploys the validated release to the operator-provided production
    folder or runtime location.
27. Release creates the GitHub Release and tag.
28. Operations Lead adapts the production operations plan (first release) or
    reviews the production readiness checklist (subsequent releases),
    establishes or updates monitoring, and begins production oversight.

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
6. Technical Lead marks triaged issues `ready-for-dev` after verifying
   required documentation and information are sufficient to begin development.
7. Normal Technical Lead-orchestrated Developer -> QA -> Release -> integration
   QA -> Operations -> manifest approval -> staged deployment cycle follows.

## Issue Status Labels

GitHub labels are used as process signals, but they are not allowed to
accumulate contradictory workflow states. For issue workflow, keep one active
workflow status label at a time:

- `draft`
- `needs-clarification`
- `ready-for-dev`
- `in-dev`
- `ready-for-qa`
- `qa-passed`
- `deferred`
- `done`

When a role advances an issue, it removes the previous workflow status label
and adds the new one in the same GitHub issue edit when possible. The owning
role still verifies the evidence required by that role's process before moving
the issue. Review and evidence labels such as `needs-architecture`,
`architecture-reviewed`, and `ops-identified` can coexist with the workflow
status when they add information, but they must be removed or replaced when
they are no longer true.

Workflow status labels:

- `draft`: issue exists but is not ready for implementation.
- `needs-clarification`: Product Owner needs input before scoping is complete.
- `ready-for-dev`: Technical Lead verified required documentation and
  information are sufficient to begin development; developer agents can pick
  up the issue.
- `in-dev`: developer agent is implementing the issue.
- `ready-for-qa`: developer pushed the branch, recorded verification and TDD
  evidence, and recorded CI evidence or a Technical Lead CI waiver record when
  CI exists.
- `qa-passed`: QA accepted the branch and required CI for the tested commit
  passed or was explicitly waived by Technical Lead using a waiver record.
- `deferred`: issue moved out of the active release.
- `done`: issue completed and included in a released version.

Review and evidence labels:

- `needs-architecture`: Architect review is required before development.
- `architecture-reviewed`: Architect recorded technical direction and
  implementation constraints.
- `ops-identified`: Operations Lead identified an operational issue in
  a managed environment.

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
