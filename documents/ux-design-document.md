# UX Design Document Specification

This specification defines structural UX artifacts. UX work describes how an
actor experiences the product; it does not define visual design, product scope,
or technical implementation.

## Artifact Budget

Every UX artifact inherits the Product Owner budget unless the operator
approves a change.

| Budget | Use | Expected length |
| --- | --- | --- |
| `inline` | Defect or issue-local UX note. | Issue comment, up to 1 page. |
| `mini` | Prototype, personal tool, terminal UX, or operational run. | 1-3 pages. |
| `standard` | Product flow or feature with real user journeys. | 4-8 pages. |
| `full` | Multi-actor, production, enterprise, regulated, or screen-heavy product. | As needed. |

Do not produce a full UX document for a terminal-only CLI, headless job, API, or
one-page script unless the operator asks. Read only the sections needed for the
selected surface and budget.

## Scope Levels

| Level | Use |
| --- | --- |
| `scoped` | Issue-local or budgeted UX note. May link to a full document. |
| `full` | Durable UX documentation for a product or major redesign. |

A scoped note may reference stable content instead of repeating it. It must
record overrides explicitly.

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
UX documentation depth:
Primary interaction mode:
Edge-case depth:
Accessibility depth:
Architecture bridge depth:
Deferred UX concerns:
```

If calibration is missing, infer only when safe and record the assumption;
otherwise return to Product Owner.

## Surface Types

- `Screen UX`: screens, navigation, wireframes, interaction states.
- `Terminal UX`: commands, flags/options, prompts, help, output, errors, exit behavior.
- `Operational UX`: job/run lifecycle, status, logs, evidence, rerun/failure behavior.
- `API Consumer UX`: operation discovery, examples, request/response, errors.
- `Report UX`: metric hierarchy, filters, freshness, exports.
- `Process UX`: actors, states, handoffs, exceptions, audit-visible transitions.
- `Agent/Handoff UX`: trigger, human checkpoints, allowed actions, evidence, escalation.

## Section Map

### Inline

1. Calibration.
2. Affected flow or command.
3. Expected actor experience.
4. Error or edge behavior.
5. Architecture implications, if any.

### Mini

1. Header and calibration.
2. Product context: one-sentence purpose and actor.
3. Primary flow: steps from entry to completion.
4. Model-specific UX:
   - CLI: command, flags/options, input validation, output, errors, exit codes, help example.
   - Operational job: start, progress/status, success evidence, failure evidence, rerun.
   - API: discoverability, examples, errors, debugging signals.
5. Accessibility or usability notes that matter to the surface.
6. Design-to-architecture bridge: only real constraints and data/evidence needs.
7. Deferred concerns.

### Standard

Use mini sections plus relevant details: user types, usage context, flow diagrams,
screen inventory, wireframes, interaction states, content priority, accessibility,
and architecture bridge.

### Full

Use standard sections plus deeper state matrices, exception paths, responsive
behavior, permissions, audit-visible states, and support handoffs when those
change downstream decisions.

## Flow Rules

Document only relevant flows. A flow records entry, completion, steps, error
paths, edge cases, data per step, and side effects. Use Mermaid when the diagram
is clearer than prose. For `mini` terminal and operational UX, a numbered flow
or command example may replace Mermaid.

## Screen UX Rules

Screen inventory, navigation model, wireframes, responsive behavior, and visual
state inventories apply only to screen-based surfaces. Do not create wireframes
for terminal-only, headless, or API-only products.

## Accessibility

Scale accessibility to the surface and assurance level. Always cover keyboard
operation for terminal/screen products, readable output and errors, focus
behavior where screens exist, and non-color status communication where visual
status exists. Add WCAG matrices only for production, enterprise, or regulated
screen UX.

## Design-to-Architecture Bridge

Record only implications the Architect needs:

- Data needed by flows.
- State changes and side effects.
- Auth or permission touchpoints.
- Offline, real-time, performance, file/media, or evidence requirements.
- Operational or support expectations visible to the actor.

Mark absent concerns as deferred rather than expanding the document.

## Output

For `standard` and `full`, produce synchronized markdown and HTML. For `inline`
and `mini`, markdown or issue text is enough unless the operator asks for HTML.
HTML must mirror markdown content without adding filler.

## Stop Conditions

Return to Product Owner for missing scope, missing users, conflicting acceptance
criteria, or feature expansion. Defer to the operator for visual design. Flag
Architect when the UX reveals technical implications the UX Lead cannot decide.
