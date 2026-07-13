Make sure your role in the process has been specified by the operator. That will guide you through the rest of the process

# Start Here

This file is the entry point for Codex, Claude, and other agent skills that
need to operate inside the Human-Codex process. Use it to identify the minimum
set of files to read before acting.

Do not start by reading the entire repository. First identify the active role,
artifact budget, target project type, product context, and technology context,
then read only the files needed for that work.

## First Questions

Before operating, make sure you understand:

- Active role: the operator must specify or confirm the role before process
  work starts.
- Requested outcome: the concrete artifact, issue, change, validation, release,
  or operational result expected.
- Artifact budget: `inline`, `mini`, `standard`, or `full`.
- Target project type: personal productivity tool, prototype, internal tool,
  production product, enterprise product, regulated product, multi-actor
  system, or another operator-specified category.
- Level of expected detail: match the artifact budget and project type. Do not
  produce full process artifacts for tiny or personal work unless the operator
  asks for them.
- Product context: the product folder, repository, issue, milestone, release,
  branch, runtime, or environment involved.
- Technology context: relevant stack guidance under `technologies/`, local
  validation commands, and tool-specific rules.
- Theme context: selected theme under `themes/` when the work creates or
  changes a visual surface.
- Environment risk: whether the task touches mutable external systems,
  credentials, deployments, runtime configuration, production, or
  non-production infrastructure.

If the role, budget, target project type, or product context is ambiguous, ask
the operator before doing process work.

## Always Read First

Read:

- `process/overview.md`

The overview defines the lifecycle, phase folders, access boundaries, artifact
budgets, and production/non-production separation rules.

## Roles

After `process/overview.md`, read the process file for the active role.

| Role | Role instructions | Agent profile |
| --- | --- | --- |
| Product Owner | `process/product-owner.md` | `agents/product-owner/` |
| UX Design Lead | `process/ux-design-lead.md` | No role profile folder currently exists |
| Architect | `process/architect.md` | `agents/architect/` |
| Technical Lead | `process/tech-lead.md` | `agents/tech-lead/` |
| Developer | `process/developer.md` | `agents/developer/` |
| QA | `process/qa.md` | `agents/qa/` |
| Release | `process/release.md` | `agents/release/` |
| Operations Lead | `process/operations-lead.md` | `agents/operations-lead/` |

When an agent profile folder exists, read its `README.md` first, then the
profile files that are relevant to the task:

- `IDENTITY.md`: role identity and responsibilities.
- `SOUL.md`: role judgment and operating principles.
- `AGENTS.md`: hard boundaries, workflow, and handoff rules.
- `TOOLS.md`: allowed tool surfaces and tool usage notes.
- `USER.md`: interaction guidance for working with the operator.
- `HEARTBEAT.md`: recurring-session checklist.

Read `TOOLS.md` when tool authority matters, especially for GitHub,
environment, runtime, release, or operational work.

## Document Specs

Read document specifications only when the active role and artifact budget call
for a standalone artifact.

- Product brief: `documents/product-brief.md`
- UX design document: `documents/ux-design-document.md`
- Architecture design document: `documents/architecture-design-document.md`
- Operations plan: `documents/operations-plan.md`

Full standalone artifacts are usually appropriate for `full` work and for
operator-requested documentation. `inline` and `mini` work may use issue notes,
checklists, or scoped docs instead.

## Supporting Context

Read supporting context only when it is relevant:

- `agents/README.md`: role profile composition and role segmentation.
- `agents/dimensions.md`: role, product, technology, and theme composition
  model.
- `process/environments.md`: required for mutable external systems,
  deployments, runtime, environment, credential, TLS, monitoring, scheduler,
  service, or production/non-production separation work.
- `technologies/README.md`: technology profile index.
- `technologies/<stack>/README.md`: stack-specific guidance for the active
  technology.
- `themes/README.md`: theme system overview for visual surfaces.
- `themes/<theme>/README.md`: selected theme guidance.
- `tools/README.md`: available tool guidance index.
- `security/README.md`: security guidance index.

## Scale The Work

Use the smallest process surface that protects the task:

- `inline`: tiny defects, small scripts, or issue-local changes. Keep records
  compact and evidence-focused.
- `mini`: prototypes, personal tools, and narrow automations. Prefer compact
  briefs, scoped technical notes, and direct validation.
- `standard`: product features or workflow changes that need downstream UX,
  architecture, planning, development, and QA coordination.
- `full`: production, enterprise, regulated, multi-actor, high-risk,
  externally distributed, or operationally owned products. Use the full role
  chain and durable evidence.

For one-off local scripts, personal CLIs, and narrow automations, default to
`mini` and Personal Productivity unless the operator asks for production-level
process or there is evidence of external users, persistent business data,
managed credentials, schedulers, daemons, compliance needs, high cost of
failure, support obligation, or operational ownership.

## Operating Rules

- Role controls authority. Product controls intent. Technology controls
  technique. Theme controls visual identity.
- Do not combine roles unless the operator explicitly asks.
- Every handoff needs operator approval or a cited delegated approval source.
- Production and non-production are separate runtime worlds. Do not rely on
  labels, route params, flags, or operator discipline as separation controls.
- Do not copy process files into product repositories. Link to or reference
  this process repository instead.
- Record required evidence before handoff: artifact or issue reference,
  branch/commit when relevant, checks run, results, known limits, approvals,
  and waivers.
