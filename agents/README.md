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
- [developer](developer/README.md)
- [qa](qa/README.md)
- [release](release/README.md)

The role operating processes live in:

- [Product Owner process](../process/product-owner.md)
- [Architect process](../process/architect.md)
- [Developer process](../process/developer.md)
- [QA process](../process/qa.md)
- [Release process](../process/release.md)

Each role folder uses the OpenClaw standard file names:

- `README.md`: what the profile contains and how to compose it.
- `IDENTITY.md`: role identity and responsibilities.
- `SOUL.md`: role-specific operating principles.
- `AGENTS.md`: hard boundaries, workflow, and handoff rules.
- `TOOLS.md`: allowed tool surfaces and tool usage notes.
- `USER.md`: role-specific interaction notes for working with the operator.
- `HEARTBEAT.md`: ops-session checklist for recurring work.

These files start from the OpenClaw default agent file structure, but they do
not copy secrets, model API keys, or shared credentials. Runtime credentials
belong in OpenClaw configuration, secret stores, or approved MCP/tool servers.

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
| Developer | Mostly yes | Carries codebase conventions, tests, local setup, and known implementation pitfalls. |
| QA | Mixed | Uses shared QA discipline, but accumulates product-specific acceptance behavior, fixtures, regressions, and test history. |
| Release | Mostly cross-product | Applies standard release mechanics across products, with product-scoped configuration and credentials. |

## Product Owner

The Product Owner is product-specific.

Responsibilities:

- Turns user briefs into GitHub Issues.
- Defines scope, out-of-scope boundaries, and acceptance criteria.
- Creates and manages GitHub Milestones.
- Owns release scope and issue priority.
- Marks issues `ready-for-dev` only when they are actionable.
- Defers issues that no longer belong in the active release.
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

## Developer

The Developer is product-bound during execution.

Responsibilities:

- Works only in the product's `dev/**` folder.
- Implements assigned GitHub Issues marked `ready-for-dev`.
- Writes and updates code-level tests.
- Records implementation notes in the GitHub Issue or pull request.
- Pushes feature branches for QA.

The Developer does not decide release scope and does not promote work into
`tst/**` or `prod/**`.

## QA

QA uses shared validation discipline but becomes product-specific through
execution.

Responsibilities:

- Reads the product's `dev/**` folder when needed.
- Works in the product's `tst/**` folder.
- Creates test checkouts from pushed branches.
- Validates acceptance criteria.
- Builds product-specific regression knowledge.
- Opens defects as GitHub Issues.
- Records validation evidence.
- Closes defects only after validation passes.

QA does not fix implementation defects in `dev/**`.

## Release

Release is centralized in process but product-scoped in execution.

Responsibilities:

- Works in the product's `prod/**` folder.
- Assembles approved issues into `release/<milestone-name>` branches.
- Promotes validated release branches to production.
- Creates GitHub Releases and tags after production promotion.
- Performs final smoke checks.
- Cleans up completed phase checkouts after release.

Release agents can serve multiple products only through product-specific release
profiles. A release profile defines the repository, branch rules, deployment
target, smoke tests, credentials, and cleanup rules for one product.

## Boundary Rules

- Product Owner memory is product memory.
- Release process is organizational memory.
- Architects, Developers, and QA use reusable skills but execute inside one
  product at a time.
- Credentials are scoped by product and role.
- No role receives write access outside its lane unless explicitly delegated.
- Cross-product agents operate through product-specific profiles, not through
  unrestricted global access.
