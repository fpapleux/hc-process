# Product Brief

This document defines the canonical product brief artifact for Product Owner
work. Use it when creating or revising a product brief for a new product,
feature, tool, or product-definition effort.

## Purpose

A product brief is a paired canonical markdown document and human-readable HTML
presentation for defining one product against a stable Product Owner structure.
It should help the Product Owner:

- Clarify product intent.
- Record calibration decisions that control interview depth and downstream
  artifact depth.
- Capture scope and non-scope.
- Surface open questions.
- Record operator design decisions.
- Identify issues that need architecture review.
- Prepare acceptance criteria or issue-ready follow-up work.

When used alongside the process Product Owner role, treat the product brief as
the main Product Owner artifact unless the user asks for a different output.

## Outputs

Create both a markdown file and a standalone HTML file in the current working
directory. Use a kebab-case filename derived from the product name:

```text
<product-name>-brief.md
<product-name>-brief.html
```

The markdown file is canonical. Agents consume and cite the markdown version
for product intent, roadmap features, acceptance criteria, constraints, open
questions, and design decisions.

The HTML file is mandatory. It is the full human-readable rendering of the
markdown brief and must open in any browser without requiring a build step.

Both versions must contain the same content at the same level of detail. The
HTML version must be created whenever the markdown version is created, and it
must be updated whenever the markdown version changes. Changing the markdown
brief without updating the HTML version is incomplete work.

## Visual System

Use the presentation skill visual system exactly:

- Embedded DM fonts.
- Sidebar and topbar layout.
- Slide navigation JavaScript.
- Reusable components.
- Mermaid setup and render-before-hide behavior.

Do not fork the presentation visual design for product briefs. Future HTML
briefs should inherit the current presentation skill design at generation time.
The visual rendering may improve readability, but it must not omit, summarize,
reinterpret, or add product intent beyond the canonical markdown content.

## Intake

Gather missing basics before generation:

- Product name.
- Tagline or one-line product framing.
- Owner, team, client, or context label.
- Existing notes, if any.
- Existing accepted product brief, if any.

Do not ask for slide names by default. The product brief structure is fixed.
Before generating or revising the brief, perform the Product Owner calibration
pass defined in `process/product-owner.md`.

## Fixed Structure

Create exactly these sections in the markdown brief and matching slides in the
HTML brief, in this order:

1. **Cover**: Product name as `cover-title`, tagline as `page-subtitle`,
   owner/context as `eyebrow`.
2. **Calibration Snapshot**: Product model, assurance level, operator
   objective, expected documentation depth, existing brief/source, interview
   depth, edge-case policy, UX depth guidance, and architecture depth guidance.
3. **Problem / Opportunity**: The gap or need the product addresses. Use cards
   for current state and why it matters. Optionally add an amber callout for the
   core pain.
4. **Target Users**: Segments or personas. Use one card per persona with role,
   needs, and goals.
5. **Value Proposition**: The core promise. Use a blue callout for the
   one-sentence value statement plus supporting copy.
6. **Solution Overview**: Key capabilities. Use capability cards.
7. **Model-Specific Definition**: Required when a defined product model applies.
   Capture only the model-specific fields that are relevant at the selected
   assurance level. For a CLI tool, include command inventory, flags/options,
   inputs, outputs, errors and exit behavior, examples, and command/function
   map. For an ETL or batch job, include source, destination, transform rules,
   trigger/schedule, validation, failure behavior, rerun behavior, and
   evidence/reporting.
8. **How It Works**: User journey, operating model, or conceptual workflow as
   a Mermaid diagram inside `.flow-diagram`. Show the experience from the
   user's perspective. Do not define system architecture, component design,
   or technical implementation — those belong to the Architect. Adjust the
   workflow to the model: CLI invocation lifecycle, ETL job/data lifecycle,
   web/app user journey, BPM process state flow, API consumer request
   lifecycle, report/dashboard decision flow, or data fix/migration validation
   flow.
9. **Differentiators**: Why this product over alternatives. Use a comparison
   table or differentiator cards.
10. **Success Metrics**: KPIs as stat cards.
11. **Roadmap**: Milestones or phases. Must include a roadmap feature table
   with three columns: `V1 features`, `V2 features`, and `Beyond features`.
   Each column lists the feature names for that phase. A Mermaid timeline or
   flow may be added, but it does not replace the required feature table.
12. **Acceptance Criteria**: Observable behavior required for acceptance.
    Criteria must reflect the selected product model and assurance level. For a
    CLI tool, include command behavior, output, errors, and exit behavior where
    relevant. For an ETL or batch job, include input/output correctness,
    validation, failure behavior, and rerun expectations where relevant.
13. **Risks & Mitigations**: Risk, impact, mitigation table with severity
    pills.
14. **Open Questions**: Running log of unknowns that must be resolved later.
    Use a table with `question`, `owner`, `needed by`, and `status`.
15. **Design Decisions**: Log of operator/product decisions that influence
    product design. Use a table with `decision`, `rationale`, `impact`, and
    `date/source`.
16. **Next Steps**: Call to action. Use a short numbered list plus a green
    callout with the recommended immediate action.

When source notes are provided, pre-fill the relevant sections/slides.
Otherwise leave clear TODO placeholders in both markdown and HTML.

## Product Owner Boundaries

The product brief defines WHAT and WHY. It must not define HOW.

The brief must include:

- Calibration snapshot with product model, assurance level, operator objective,
  interview depth, and downstream UX/architecture depth guidance.
- User goals and business outcomes.
- Scope and out-of-scope boundaries.
- Acceptance criteria stated as observable behavior, not implementation.
- Roadmap feature inventory with V1, V2, and Beyond feature lists. Give each
  listed feature a clear name that can be referenced later by GitHub Issues.
- Business-driven constraints that have technical implications (state the
  constraint and the business reason, not the solution).
- Target user descriptions.
- Model-specific definition when a defined model such as CLI tool or ETL /
  batch job applies.

The brief must NOT include:

- Technology choices (languages, frameworks, databases, cloud services).
- Component design or system decomposition.
- Data model, schema, or storage decisions.
- Integration patterns or API design.
- Deployment approach or infrastructure decisions.
- Security implementation details (state compliance requirements; the Architect
  designs the controls).

When the Product Owner has technical context from prior experience, record it
in the "Design Decisions" section/slide explicitly labeled as context for the
Architect, not as a requirement.

The Product Owner must not implement code, create PRs, mutate production,
deploy, or perform QA validation.

## Mermaid Rules

Brief diagrams must favor legibility over density.

- Use Mermaid for flows, timelines, architecture, and sequence-style diagrams.
- Lay out boxes as if drawn on a grid: align related nodes horizontally and
  vertically where possible.
- Prefer vertical diagrams when the node count is more than a few boxes.
- Avoid diagonal connections where possible.
- Prefer top-down or left-right flows with stair-step-like connections made from
  vertical and horizontal segments.
- Avoid long rows of horizontally aligned boxes. Mermaid may shrink text to fit,
  so dense horizontal layouts can make labels unreadable.
- Use concise node labels and move detailed explanation into slide text, cards,
  or tables.
- If a flow becomes crowded, split it across multiple diagrams or use phases.

## Handoff

When the product brief is accepted by the operator, the next step in the
process is the UX Design Lead. The UX Design Lead reads the accepted product
brief and produces the UX design document (user flows, wireframes, interaction
specs, accessibility requirements). The product brief is also consumed later by
the Architect alongside the approved UX designs.

Downstream roles use the accepted markdown brief as the canonical source. The
HTML brief is the required human-readable version for review and operator
discussion.

The product brief must be complete enough for the UX Design Lead to identify
all user types, goals, journeys, and constraints without guessing. If the brief
does not describe who the users are or what they need to accomplish, it is not
ready for handoff.

The brief must also be calibrated enough for downstream roles to know how much
work is expected. A prototype brief should not force production-grade UX,
architecture, or development planning. A production-grade or enterprise brief
must not hide material ambiguity behind prototype-style shortcuts.

## Completion Message

After creating or revising a product brief, tell the user:

- The markdown filename.
- The HTML filename.
- That the HTML version opens in any browser.
- Whether DM fonts were embedded.
- That the fixed product brief structure was created or updated in both files.
- That the HTML version matches the markdown content.
- The selected product model, assurance level, and documentation depth.
- Which sections/slides were pre-filled versus left as TODOs.
- Suggested next edits, such as filling open questions, adding personas, turning
  a roadmap into a Mermaid timeline, or adding success metrics.
