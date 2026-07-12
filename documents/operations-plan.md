# Operations Plan Document Specification

This specification defines operations plans for systems that are actually
operated. Do not create an operations plan for a one-off local script,
prototype, personal CLI, or headless helper unless it has a maintained runtime,
production users, scheduler/daemon ownership, external availability promise,
backup/restore need, managed credentials/TLS, monitoring requirement, or an
operator request.

## Artifact Budget

| Budget | Use | Expected length |
| --- | --- | --- |
| `none` | No operated runtime. Record why no plan is needed. | One line in architecture note. |
| `inline` | Release/incident operations note. | Issue/comment, up to 1 page. |
| `mini` | Small maintained job/tool with limited operator needs. | 1-3 pages. |
| `standard` | Production service or scheduled job. | 4-8 pages. |
| `full` | Enterprise, regulated, multi-service, or high-availability system. | As needed. |

Read only the sections needed for the budget.

## Scope Levels

| Level | Use |
| --- | --- |
| `scoped` | Release, incident, or specific operational change. |
| `full` | Durable plan for a production or maintained runtime. |

Scoped notes may reference a full plan instead of repeating stable content.

## Header

```text
Product:
Scope level:
Budget:
Date:
Author:
Status: draft | review | accepted | superseded
Canonical path:
Architecture document:
Related issues:
```

## Authorship

The Architect creates the initial plan only when the trigger exists. The
Operations Lead adapts it after first production release and owns it thereafter.
Sections needing production data may be placeholders until adaptation.

## Section Map

### None

Record: `Operations plan: not required because <reason>. Revisit if the tool
gets a maintained runtime, external users, scheduler, credentials, backups, or
SLOs.`

### Inline

1. Operational change or incident context.
2. Affected runtime/environment.
3. Required action or runbook change.
4. Verification evidence.
5. Follow-up issue, if needed.

### Mini

1. Header and service overview.
2. How to run, stop, restart, or rerun.
3. Expected success/failure signals.
4. Logs/evidence location.
5. Credentials or runtime material, without secret values.
6. Backup/restore or recovery note, if applicable.
7. Escalation/contact.

### Standard

Use mini sections plus SLIs/SLOs, dashboards, alerts, incident procedure,
capacity watchpoints, deployment smoke, backup/restore, security monitoring,
and maintenance cadence.

### Full

Use standard sections plus error-budget policy, escalation matrix, post-incident
review process, dependency runbooks, compliance evidence, access reviews,
capacity planning, disaster recovery, and audit requirements.

## Required Content Rules

- Every metric must answer an operational question or support an SLI, alert,
  capacity concern, or troubleshooting path.
- Every alert must be actionable and have a response procedure.
- Do not invent dashboards, SLOs, backups, or on-call procedures for systems
  that do not need them.
- Never include secret values. Record secret roots, owners, rotation, and access
  paths only at the safe level of detail.
- Production/non-production separation and credential safety are never waived.

## Output

For `standard` and `full`, produce synchronized markdown and HTML. For `none`,
`inline`, and `mini`, markdown or issue text is enough unless the operator asks
for HTML. HTML must mirror markdown content without adding filler.

## Adaptation Workflow

After first production release, Operations Lead verifies actual baselines,
dashboards, alerts, restore evidence, credential ownership, runbooks, and smoke
procedures. Replace placeholders with observed values and mark the plan
`accepted` only when operational evidence exists.
