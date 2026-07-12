# Agent Roles

This folder defines the agent profiles used by this product. Each profile must
state its scope, permissions, inputs, outputs, and handoff rules.

The roles are separated so that product intent, development, validation, and
release authority do not collapse into one agent.

Agent profiles are composed from three dimensions:

- Role: what authority the agent has.
- Product: what product context the agent carries.
- Technology: what technical practices the agent follows.

See [Profile Dimensions](dimensions.md) for the full model.

## Role Profile Folders

The standard role profiles live in:

- [product-owner](product-owner/README.md)
- [architect](architect/README.md)
- [tech-lead](tech-lead/README.md)
- [developer](developer/README.md)
- [qa](qa/README.md)
- [release](release/README.md)
- [operations-lead](operations-lead/README.md)

The role operating processes live in:

- [Product Owner process](../process/product-owner.md)
- [Architect process](../process/architect.md)
- [Technical Lead process](../process/tech-lead.md)
- [Developer process](../process/developer.md)
- [QA process](../process/qa.md)
- [Release process](../process/release.md)
- [Operations Lead process](../process/operations-lead.md)

Each role folder uses standard profile file names:

- `README.md`: what the profile contains and how to compose it.
- `IDENTITY.md`: role identity and responsibilities.
- `SOUL.md`: role-specific operating principles.
- `AGENTS.md`: hard boundaries, workflow, and handoff rules.
- `TOOLS.md`: allowed tool surfaces and tool usage notes.
- `USER.md`: role-specific interaction notes for working with the operator.
- `HEARTBEAT.md`: ops-session checklist for recurring work.

These files do not copy secrets, model API keys, or shared credentials. Runtime
credentials belong in runtime configuration, secret stores, or approved
MCP/tool servers.

## Context Model

Some agent knowledge belongs to the organization and can be reused across
products. Other knowledge belongs to a specific product and accumulates over
time.

Organizational context:

- Release process.
- QA standards.
- GitHub label and status conventions.
- Repository templates.
- Security policy.
- Agent role definitions.

Product-specific context:

- Product vision and scope.
- Roadmap and priorities.
- User workflows.
- Naming and domain language.
- Codebase architecture and conventions.
- Fixtures, test data, and regression history.
- Deployment configuration.

## Role Segmentation

| Role | Product Specific | Reason |
| --- | --- | --- |
| Product Owner | Yes | Carries product vision, priorities, scope boundaries, roadmap, naming, tradeoffs, and the meaning of done. |
| Architect | Yes | Carries product architecture, system boundaries, technical decisions, integration risks, and durable design constraints. |
| Technical Lead | Yes | Carries delivery plan, issue sequencing, milestone/release coordination, environment readiness, process enforcement, and defect backlog context. |
| Developer | Mostly yes | Carries codebase conventions, tests, local setup, and known implementation pitfalls. |
| QA | Mixed | Uses shared QA discipline, but accumulates product-specific acceptance behavior, fixtures, regressions, and test history. |
| Release | Mostly cross-product | Applies standard release mechanics across products, with product-scoped configuration and credentials. |
| Operations Lead | Mostly cross-product | Applies standard environment, monitoring, incident, and runbook discipline with product-scoped infrastructure and secrets. |

## Product Owner

The Product Owner is product-specific.

Responsibilities:

- Turns user briefs into GitHub Issues.
- Maintains the roadmap feature inventory for V1, V2, and Beyond.
- Defines scope, out-of-scope boundaries, and acceptance criteria.
- Owns product priority, release contents, and included/deferred issue
  decisions.
- Ensures issue documentation and product information are sufficient for
  Technical Lead readiness review.
- Records deferral decisions for issues that no longer belong in the active
  release.
- Carries long-term product memory.

The Product Owner does not implement code.

## Architect

The Architect is product-specific and technology-aware.

Responsibilities:

- Reviews technically significant issues before development starts.
- Records architecture decisions, implementation constraints, and technical
  risks.
- Identifies affected boundaries, data, dependencies, security, reliability,
  and operational concerns.
- Reviews pull requests for architecture fit when required.
- Hands implementation constraints to Developer and validation focus to QA.

The Architect does not implement code, decide product priority, validate QA, or
promote releases.

## Technical Lead

The Technical Lead is product-specific and delivery-focused.

Responsibilities:

- Translates accepted product, UX, and architecture inputs into executable
  GitHub Issues.
- Maintains issue hygiene, sequencing, dependencies, and status integrity.
- Arranges missing release and milestone records when activated.
- Facilitates Product Owner decisions about which issues are included in the
  next release.
- Prepares and maintains development, testing, staging, release rehearsal, and
  maintained non-production readiness within Technical Lead authority.
- Coordinates Developer, QA, Release, and Operations handoffs.
- Owns defect backlog triage and maintenance release planning.
- Enforces TDD evidence before Developer work advances to QA.
- Executes post-production cleanup of completed feature branches and checkouts
  from `dev/` and `tst/`.

The Technical Lead does not define product scope, make architecture decisions,
write application code, run QA acceptance tests, assemble release branches, or
promote releases.

## Developer

The Developer is product-bound during execution.

Responsibilities:

- Works only in the product's `dev/**` folder.
- Implements assigned GitHub Issues marked `ready-for-dev`.
- Drives implementation with TDD: writes or updates the focused test first,
  records the expected failing run, implements the minimal passing change, and
  records the passing rerun.
- Records implementation notes in the GitHub Issue or pull request.
- Pushes feature branches for QA.

The Developer does not decide release contents and does not promote work into
`tst/**` or `rel/**`.

## QA

QA uses shared validation discipline but becomes product-specific through
execution.

Responsibilities:

- Reads the product's `dev/**` folder when needed.
- Works in the product's `tst/**` folder.
- Creates test checkouts from pushed branches.
- Validates acceptance criteria.
- Checks that Developer TDD evidence exists for changed behavior and defect
  fixes before marking work `qa-passed`.
- Builds product-specific regression knowledge.
- Opens defects as GitHub Issues.
- Records validation evidence.
- Closes defects only after validation passes.

QA does not fix implementation defects in `dev/**`.

## Release

Release is centralized in process but product-scoped in execution.

Responsibilities:

- Works in the product's local release source checkouts under `rel/**`.
- Assembles approved issues into `release/<milestone-name>` branches.
- Promotes validated release branches to `main` in source control and deploys
  to the operator-provided production target.
- Creates GitHub Releases and tags after production promotion.
- Performs final smoke checks.
- Cleans up only Release-owned `rel/**` workspaces after release and hands
  `dev/**` and `tst/**` cleanup candidates to Technical Lead.

Release agents can serve multiple products only through product-specific release
profiles. A release profile defines the repository, branch rules, deployment
target, smoke tests, credentials, and cleanup rules for one product.

## Operations Lead

Operations Lead is centralized in process but product-scoped in execution.

Responsibilities:

- Provisions and maintains production environment material.
- Owns production secret roots, scoped credentials, TLS material,
  service-manager setup, runtime directories, dashboards, alerts, and runbooks
  outside Git.
- Monitors production managed environments and records operational issues.
- Maintains the operations plan after activation.
- Hands approved production environment material to Release without exposing
  secret values.

Operations Lead agents can serve multiple products only through product-specific
operations profiles. An operations profile defines the production
environments, secret locations, service-manager rules, monitoring targets,
incident procedures, and runbooks for one product.

## Boundary Rules

- Product Owner memory is product memory.
- Release process is organizational memory.
- Architects, Developers, and QA use reusable skills but execute inside one
  product at a time.
- Credentials are scoped by product and role.
- No role receives write access outside its lane unless explicitly delegated.
- Cross-product agents operate through product-specific profiles, not through
  unrestricted global access.
