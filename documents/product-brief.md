# Product Brief

This specification defines Product Owner briefs. A brief records product intent,
not implementation. Scale the artifact to the calibrated budget.

## Artifact Budget

Every brief declares one budget:

| Budget | Use | Expected length |
| --- | --- | --- |
| `inline` | Defect, tiny script, or issue-local change. | GitHub issue/comment, up to 1 page. |
| `mini` | Prototype or personal tool with narrow scope. | 1-3 pages. |
| `standard` | Product or feature that needs downstream UX/architecture. | 4-8 pages. |
| `full` | Production, enterprise, regulated, or multi-actor product. | As needed, but no filler. |

Default new one-off local scripts and personal CLI tools to `mini` unless the
operator asks for production readiness, distribution, external users,
compliance, persistent data, environment management, or operational ownership.
Do not inflate the budget because a full template exists.

## Outputs

For `standard` and `full`, create synchronized markdown and HTML files:

```text
<product-name>-brief.md
<product-name>-brief.html
```

For `inline` and `mini`, markdown is enough unless the operator asks for HTML.
When HTML is produced, it must match the markdown content. Use the presentation
visual system; do not add content just to fill slides.

## Intake

Gather only missing basics before generation:

- Product name and one-line framing.
- Owner, client, or context label.
- Existing notes or accepted brief.
- Product model, assurance level, operator objective, and budget.

For new product definition, do not use `Open Questions` as a replacement for
interview. If the calibrated happy path is unclear, ask the minimum necessary
questions first. For a CLI tool, capture the operator goal, primary command,
inputs/options/defaults, expected output, and common failure behavior.

Apply the Product Owner low-value clarification filter. Resolve low-impact
ambiguity with a documented default when it does not change scope, acceptance,
safety, trust, or downstream routing. Treat one policy-level answer as covering
related details.

## Section Map

Read only the sections needed for the selected budget and product model.

### Inline

Use this structure inside the owning issue or comment:

1. Context.
2. Product model, assurance level, operator objective, and budget.
3. Scope and out of scope.
4. Acceptance criteria.
5. Open questions or assumptions.

### Mini

Use this structure for prototypes, personal tools, and narrow CLI utilities:

1. Cover: product name, owner/context, one-line purpose.
2. Calibration: product model, assurance level, objective, budget, interview
   depth, edge-case policy, downstream depth.
3. Problem and value: why this is useful now.
4. Users: only real user types known from context.
5. Scope: in, out, defaults, assumptions.
6. Model-specific definition: for CLI, command, inputs, output, errors, examples.
7. Acceptance criteria: observable behavior.
8. Open questions and design decisions: only unresolved or chosen items.
9. Next step.

### Standard And Full

Use the mini sections plus the sections that materially help downstream roles:

- Solution overview.
- User journey or operating model diagram.
- Roadmap table with `V1`, `V2`, and `Beyond` features.
- Risks and mitigations.
- Success metrics.

For `full`, add deeper target-user, release, compliance, or business-constraint
detail only where it changes decisions.

## Boundaries

The brief defines what and why. It must include user goals, business outcomes,
scope, out-of-scope boundaries, acceptance criteria, business constraints, and
model-specific behavior. It must not prescribe technology, components, data
models, integration patterns, deployment, infrastructure, or security controls.
Technical context may be recorded as "Context for Architect", not as a
requirement.

## Diagrams

Use Mermaid only when it clarifies a workflow or roadmap. Prefer one readable
diagram over several dense diagrams. Skip diagrams for `inline` work and for
`mini` CLI tools when a command example is clearer.

## Handoff

A brief is ready only when it is calibrated enough for downstream roles to keep
the same budget. A mini brief should not force standard UX, architecture, or
operations artifacts. A full brief must not hide material ambiguity.

## Completion Message

Report filenames, whether HTML was produced, selected product model, assurance
level, budget, pre-filled sections, TODOs, and the recommended next action.
