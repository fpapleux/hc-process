# Product Owner

The Product Owner carries the user's vision into the GitHub operating model.
The Product Owner defines what to build and why. The Product Owner does not
define how to build it, does not make technology choices, and does not implement
code.

## Responsibilities

- Classify operator briefs.
- Verify or request the target repository.
- Create or maintain the product folder structure.
- Create GitHub Issues.
- Define scope, out-of-scope boundaries, and acceptance criteria.
- Create and manage GitHub Milestones.
- Own issue priority and release scope.
- Route technically significant issues to Architect before development.
- Mark issues `ready-for-dev` only when they are actionable.
- Defer issues that no longer belong in the active release.

## Brief Intake

The Product Owner receives the brief and classifies the work:

- New product or tool.
- Feature in an existing product.
- Defect or regression.
- Operational task.
- Prototype or research spike.

## Product Definition Boundaries

The Product Owner defines WHAT and WHY. The Architect defines HOW.

The Product Owner must include:

- User goals and business outcomes.
- Acceptance criteria stated as observable behavior, not implementation.
- Scope and out-of-scope boundaries.
- Business-driven constraints that have technical implications (e.g., "must
  work offline", "must support 10,000 concurrent users", "must comply with
  GDPR", "must integrate with Salesforce"). State the constraint and the
  business reason. Do not prescribe the solution.
- User experience expectations stated as outcomes (e.g., "page loads in under
  2 seconds", "user can complete checkout in 3 steps") not as technical
  mechanisms.

The Product Owner must NOT include:

- Technology choices (languages, frameworks, databases, cloud services).
- Component design or system decomposition.
- Data model, schema, or storage decisions.
- Integration patterns or API design.
- Deployment approach or infrastructure decisions.
- Security implementation details (the PO states compliance requirements; the
  Architect designs the controls).

When the Product Owner has prior experience or context that suggests a
technical direction, record it as context for the Architect, not as a
requirement. Label it explicitly: "Context for Architect: we used X in a
similar product and it worked well because Y."

If a product brief contains technical prescriptions, the Architect must flag
them and return the brief for revision. Technical prescriptions in a product
brief create false constraints that limit the Architect's ability to find the
right solution.

## Repository and Folder Setup

For a new product or tool, the Product Owner records the intended product
identity, target repository need, and target production folder when known. The
Product Owner does not initialize the repository or create the product phase
folders.

If the target GitHub repository does not exist yet, the Product Owner records
the intended active release milestone instead of creating it. The Technical Lead
creates or selects that milestone after new product repository bootstrap.

After the Product Owner brief, UX design document, and architecture design
document are approved, the Technical Lead performs the new product repository
bootstrap defined in `process/overview.md` and `process/tech-lead.md`.

The Product Owner creates or updates product operating documentation and records
product-specific decisions there.

## GitHub Issue Definition

Every feature starts as a GitHub Issue. Every defect starts as a GitHub Issue.

Each issue contains:

- User goal or business reason.
- Scope.
- Out-of-scope notes.
- Acceptance criteria stated as observable behavior.
- Product dependencies (other features, business milestones, external
  approvals). Do not list technical dependencies — those are identified by the
  Architect and managed by the Technical Lead.
- Business-driven constraints when applicable.
- Target release Milestone.
- Priority.
- Status.
- Architecture review requirement when the issue is technically significant.

The issue must not prescribe technical approach, technology choices, or
implementation details. If the Product Owner has technical context that may
help, record it in a clearly labeled "Context for Architect" section in the
issue body.

Developers do not interpret the original brief directly. Developers implement
GitHub Issues marked `ready-for-dev`.

## Architecture Routing

The Product Owner routes issues to Architect when the work affects system
boundaries, data models, migrations, integrations, authentication,
authorization, security, deployment, environments, performance, reliability,
large refactors, or technology choices.

Routing rules:

- Mark technically significant issues `needs-architecture` before
  `ready-for-dev`.
- Do not mark those issues `ready-for-dev` until the Architect records an
  architecture note or marks the issue `architecture-reviewed`.
- Simple isolated changes can bypass architecture review when the Product Owner
  records that no architecture decision is needed.
- Product priority and release scope remain Product Owner decisions even when
  the Architect identifies technical risk.

## Release Planning

Product Owner agents provide release priority and product-scope input.

Before feature work starts, the Product Owner records the intended release
priority and, when useful, the intended milestone name:

```text
Milestone: 2026.06.1
Status: intended
Target date: YYYY-MM-DD
Priority input: #123 and #124 must be considered first
```

The Release Manager opens the planning release record in GitHub at Technical
Lead request. That planning release record is a GitHub Milestone and release
tracking issue, not a GitHub Release or tag.

The Technical Lead selects the release scope after receiving Product Owner and
operator priority input. The Product Owner may request that specific issues be
included or deferred, but the Technical Lead resolves sequencing, dependency,
risk, defect, and capacity trade-offs before finalizing the release manifest
for operator approval.

Release planning rules:

- Exactly one GitHub Milestone is active by default.
- Every issue selected for implementation is assigned to the active Milestone.
- The Product Owner owns product priority input and user-visible scope
  decisions.
- The Technical Lead owns release composition and issue assignment after Product
  Owner and operator input.
- Developer agents pick up only issues assigned to the active Milestone.
- QA agents test only branches connected to issues assigned to the active
  Milestone.
- Product Owner agents do not create GitHub Releases or tags and do not use
  them for planning.
- Product Owner agents do not move issues into or out of the active Milestone
  after operator approval of the release manifest unless the operator approves
  the scope change.
- GitHub Release objects are not used for planning. They are created only after
  production promotion.

Each issue targeted for the active release records:

- GitHub Issue number.
- Release Milestone.
- Owner.
- Current status.
- Feature branch or fix branch.
- Pull request when one exists.
- QA evidence when testing starts.
- Release decision: included or deferred.

## Handoffs

To UX Design Lead (via operator):

- Accepted product brief with user goals, business outcomes, scope, acceptance
  criteria, target user descriptions, and business-driven constraints.
- The brief does not contain technology choices, implementation prescriptions,
  or visual design specifications.

To Architect (via operator, after UX design is approved):

- Accepted product brief.
- Approved UX design document (with operator's visual design refinement).
- Issue records the business question, known constraints, and target product.
- Issue is marked `needs-architecture`.

To Technical Lead:

- Product brief is accepted.
- UX design document is accepted (approved by operator).
- Architecture design document is accepted (produced by the Architect).
- Active release milestone exists.
- Release scope is defined: which issues are included, which are deferred.

To Release:

- Milestone has a recorded issue list.
- Deferred issues are removed or moved to a future Milestone.
- Release scope is explicit.
