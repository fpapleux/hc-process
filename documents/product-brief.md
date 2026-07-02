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
6. **How It Works**: Architecture, operating model, or user flow as a Mermaid
   diagram inside `.flow-diagram`.
7. **Differentiators**: Why this product over alternatives. Use a comparison
   table or differentiator cards.
8. **Success Metrics**: KPIs as stat cards.
9. **Roadmap**: Milestones or phases. Use a table or Mermaid timeline/flow.
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

When operating as Product Owner, the brief may define product scope and follow-up
work, but the Product Owner must not implement code, mutate production, deploy,
or perform QA validation.

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

## Completion Message

After creating or revising a product brief, tell the user:

- The filename.
- That it opens in any browser.
- Whether DM fonts were embedded.
- That the fixed product brief structure was created or updated.
- Which slides were pre-filled versus left as TODOs.
- Suggested next edits, such as filling open questions, adding personas, turning
  a roadmap into a Mermaid timeline, or adding success metrics.
