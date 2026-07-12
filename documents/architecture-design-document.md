# Architecture Design Document Specification

This specification defines architecture artifacts. Architecture explains the
technical direction needed for the calibrated work; it does not implement code,
restate product intent, or expand scope.

## Artifact Budget

Every architecture artifact inherits the Product Owner and UX budget unless the
operator approves a change.

| Budget | Use | Expected length |
| --- | --- | --- |
| `inline` | No architecture decision or a tiny issue-local note. | Issue comment, up to 1 page. |
| `mini` | Prototype or personal tool with one boundary. | 1-3 pages. |
| `standard` | Product/feature needing durable technical direction. | 4-8 pages. |
| `full` | Production, enterprise, regulated, multi-service, data-sensitive, or operationally owned system. | As needed. |

Do not produce a full architecture document for a one-page script, personal CLI,
terminal-only helper, or scoped defect unless the operator asks or the work has
production users, persistent state, credentials, destructive actions,
compliance, external integrations, deployment ownership, or material security
risk. Read only the sections needed for the selected budget.

## Scope Levels

| Level | Use |
| --- | --- |
| `scoped` | Issue-local architecture note. May reference a full document. |
| `full` | Durable product, system, or component architecture. |

Scoped notes must not repeat stable content or contradict a full document
without recording the override and rationale.

## Header

Each artifact starts with:

```text
Product:
Scope level:
Budget:
Date:
Author:
Status: draft | review | accepted | superseded
Canonical path:
Related issues:
Related documents:
```

## Calibration

Record:

```text
Product model:
Assurance level:
Operator objective:
UX surface type:
Architecture scope:
Architecture documentation depth:
Risk depth:
Security depth:
Operations depth:
Verification depth:
Theme integration depth:
Deferred architecture concerns:
```

If calibration is missing, infer only when safe and record the assumption;
otherwise return to the owning role.

## Assurance Defaults

- `Prototype`: smallest technical shape for the happy path while respecting hard safety boundaries.
- `Personal Productivity`: simple maintainable structure for one operator or small known group.
- `Production Grade`: reliable, configurable, observable, supportable architecture.
- `Enterprise / Regulated`: auditability, roles, compliance, support handoffs, governance.

Production-grade depth requires evidence: external users, production runtime,
distribution, persistent business data, security/compliance requirements,
operational ownership, high cost of failure, or explicit operator request.

## Model Focus

- `CLI Tool`: execution context, command dispatch, inputs/outputs, filesystem or network access, credentials, idempotency, logging, destructive-operation safeguards, verification.
- `ETL / Batch Job`: source/destination boundaries, transform ownership, credentials, volume, idempotency, checkpoint/replay, validation, monitoring, recovery.
- `API / Service`: consumers, contracts, auth boundaries, error contract, compatibility, observability, operational support.
- `Workflow / BPM Platform`: state ownership, permissions, audit-visible transitions, exceptions, integrations, recovery.

Use only the relevant focus items. Mark excluded production concerns as
deferred instead of designing them.

## Section Map

### Inline

1. Decision or "no architecture review required".
2. Reason.
3. Constraints or verification notes.
4. Deferred concerns.

### Mini

1. Header and calibration.
2. Context and scope: one paragraph, system boundary if non-obvious.
3. Key decisions: each with decision, rationale, consequence.
4. Runtime/dependency shape: local files, APIs, credentials, packages, or none.
5. Data and security notes: only real data, secrets, permissions, destructive actions.
6. Verification expectations.
7. Deferred concerns and handoff notes.

### Standard

Use mini sections plus relevant component boundaries, integration contracts,
data ownership, deployment/environment notes, risk table, capability targets,
and operations expectations.

### Full

Use standard sections plus deeper context diagrams, quality attributes, threat
model, migration/deployment topology, observability, recovery, capacity,
operations plan, and compliance details where they change decisions.

## Decision Format

Use this compact format for each decision:

| Decision | Rationale | Consequence | Status |
| --- | --- | --- | --- |

Do not include generic principles, catalogs, or pattern lists unless a specific
principle or pattern changes an actual decision.

## Diagrams

Use Mermaid only when it improves understanding. One context or component
diagram is enough for most `mini` or `standard` work. Label trust boundaries
when data or control crosses zones with different trust levels.

## Security

Security depth scales, but hard safety boundaries do not. Always record secrets,
credentials, sensitive data, destructive actions, production/non-production
separation, and permission assumptions when they exist. If none exist, say so.
Do not invent controls for absent risks.

## Verification

State the test-first expectations and checks developers or QA need:
smoke/manual, focused automated checks, contract tests, migration checks,
security checks, or operational smoke. For developer-owned behavior changes,
identify the focused behavior or regression test that should fail before
implementation when architecture context affects the test shape. The
verification plan must match the budget and risk.

When the selected theme affects architecture, record the relevant checks:
asset delivery, bundle impact, rendering constraints, accessibility contracts,
or platform limitations. Do not expand architecture review for visual styling
that can be handled entirely by UX, Developer, and QA.

## Operations Plan Trigger

An operations plan is required only when there is a production or maintained
runtime, service ownership, scheduler, daemon, external users, SLO/alerting
need, backup/restore obligation, credentials/TLS managed outside Git, or an
operator request. For prototypes and personal scripts, record "operations plan:
not required" with the reason.

## Output

For `standard` and `full`, produce synchronized markdown and HTML. For `inline`
and `mini`, a markdown note or issue comment is enough unless the operator asks
for HTML. HTML must mirror markdown content and must not add filler.

## Handoff

Provide only what the next role needs: constraints, decisions, dependencies,
test-first expectations, verification expectations, deferred concerns, and
whether operations material is required. Return to Product Owner or UX when
scope or experience is unclear.
