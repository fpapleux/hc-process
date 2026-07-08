# Product Brief

This document defines the canonical product brief artifact for Product Owner
work. Use it when creating or revising a product brief for a new product,
feature, tool, or product-definition effort.

## Purpose

A product brief is a standalone presentation for defining one product against a
stable Product Owner structure. It should help the Product Owner:

- Clarify product intent.
- Capture scope and non-scope.
- Surface open questions.
- Record operator design decisions.
- Identify issues that need architecture review.
- Prepare acceptance criteria or issue-ready follow-up work.

When used alongside the process Product Owner role, treat the product brief as
the main Product Owner artifact unless the user asks for a different output.

## Output

Create a single standalone `.html` file in the current working directory. Use a
kebab-case filename derived from the product name, ending in `-brief.html`.

The artifact must open in any browser without requiring a build step.

## Visual System

Use the presentation skill visual system exactly:

- Embedded DM fonts.
- Sidebar and topbar layout.
- Slide navigation JavaScript.
- Reusable components.
- Mermaid setup and render-before-hide behavior.

Do not fork the presentation visual design for product briefs. Future briefs
should inherit the current presentation skill design at generation time.

## Intake

Gather missing basics before generation:

- Product name.
- Tagline or one-line product framing.
- Owner, team, client, or context label.
- Existing notes, if any.

Do not ask for slide names by default. The product brief structure is fixed.

## Fixed Slide Structure

Create exactly these slides, in this order:

1. **Cover**: Product name as `cover-title`, tagline as `page-subtitle`,
   owner/context as `eyebrow`.
2. **Problem / Opportunity**: The gap or need the product addresses. Use cards
   for current state and why it matters. Optionally add an amber callout for the
   core pain.
3. **Target Users**: Segments or personas. Use one card per persona with role,
   needs, and goals.
4. **Value Proposition**: The core promise. Use a blue callout for the
   one-sentence value statement plus supporting copy.
5. **Solution Overview**: Key capabilities. Use capability cards.
6. **How It Works**: User journey, operating model, or conceptual workflow as
   a Mermaid diagram inside `.flow-diagram`. Show the experience from the
   user's perspective. Do not define system architecture, component design,
   or technical implementation — those belong to the Architect.
7. **Differentiators**: Why this product over alternatives. Use a comparison
   table or differentiator cards.
8. **Success Metrics**: KPIs as stat cards.
9. **Roadmap**: Milestones or phases. Must include a roadmap feature table
   with three columns: `V1 features`, `V2 features`, and `Beyond features`.
   Each column lists the feature names for that phase. A Mermaid timeline or
   flow may be added, but it does not replace the required feature table.
10. **Risks & Mitigations**: Risk, impact, mitigation table with severity
    pills.
11. **Open Questions**: Running log of unknowns that must be resolved later.
    Use a table with `question`, `owner`, `needed by`, and `status`.
12. **Design Decisions**: Log of operator/product decisions that influence
    product design. Use a table with `decision`, `rationale`, `impact`, and
    `date/source`.
13. **Next Steps**: Call to action. Use a short numbered list plus a green
    callout with the recommended immediate action.

When source notes are provided, pre-fill the relevant slides. Otherwise leave
clear TODO placeholders.

## Product Owner Boundaries

The product brief defines WHAT and WHY. It must not define HOW.

The brief must include:

- User goals and business outcomes.
- Scope and out-of-scope boundaries.
- Acceptance criteria stated as observable behavior, not implementation.
- Roadmap feature inventory with V1, V2, and Beyond feature lists. Give each
  listed feature a clear name that can be referenced later by GitHub Issues.
- Business-driven constraints that have technical implications (state the
  constraint and the business reason, not the solution).
- Target user descriptions.

The brief must NOT include:

- Technology choices (languages, frameworks, databases, cloud services).
- Component design or system decomposition.
- Data model, schema, or storage decisions.
- Integration patterns or API design.
- Deployment approach or infrastructure decisions.
- Security implementation details (state compliance requirements; the Architect
  designs the controls).

When the Product Owner has technical context from prior experience, record it
in the "Design Decisions" slide explicitly labeled as context for the Architect,
not as a requirement.

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

The product brief must be complete enough for the UX Design Lead to identify
all user types, goals, journeys, and constraints without guessing. If the brief
does not describe who the users are or what they need to accomplish, it is not
ready for handoff.

## Completion Message

After creating or revising a product brief, tell the user:

- The filename.
- That it opens in any browser.
- Whether DM fonts were embedded.
- That the fixed product brief structure was created or updated.
- Which slides were pre-filled versus left as TODOs.
- Suggested next edits, such as filling open questions, adding personas, turning
  a roadmap into a Mermaid timeline, or adding success metrics.
