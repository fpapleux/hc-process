# Development Plan

This specification defines the Technical Lead's development plan: the synthesis
that turns an accepted product brief, UX design document, and architecture
design document into an executable, sequenced, release-oriented breakdown of
work. It is the single orientation document for every role and subagent working
the product.

The Technical Lead owns and maintains the development plan. It is saved in the
product's `documentation/` folder and is cumulative: each release adds a section
rather than replacing the prior plan, so the plan grows into the full delivery
history of the product.

## Why It Exists

A list of GitHub Issues tells an agent what to do next; it does not tell the
agent how the work fits together. The development plan gives every role — and
especially a spawned subagent working one narrow item — the whole picture: what
the release delivers, how items depend on each other, what can run in parallel,
and where a given item fits. Agents are more effective when their scoped work is
anchored in that shared synthesis.

## When It Is Required

- Required for any feature work that involves more than one role, even at
  reduced depth.
- Skippable for `inline` defect fixes and tiny single-role, issue-local changes;
  an issue note carries the same information at that scale.
- `mini` multi-role work uses a short-form plan: overview plus a work-item list
  with scope, success criteria, and definition of done, plus a simple sequence.
- `standard` and `full` work use the full plan with release organization,
  parallelization tracks, and synchronized markdown and HTML.

## Artifact Budget

| Budget | Form |
| --- | --- |
| `inline` | Not required; use the issue. |
| `mini` | Short-form markdown plan (required when more than one role is involved). |
| `standard` | Full markdown plan; HTML when the operator asks. |
| `full` | Synchronized markdown and HTML. |

## Inputs

All three must be in `accepted` status before planning starts. If any is in
`draft` or `review`, return it to its owner.

- Product brief: roadmap features, scope, acceptance criteria, and calibration.
- UX design document: flows, states, surfaces, and design-to-architecture bridge.
- Architecture design document: components, constraints, verification
  expectations, and intentional deferrals.

## Outputs

Save in the product's `documentation/` folder:

```text
documentation/<product-name>-development-plan.md
documentation/<product-name>-development-plan.html
```

The markdown version is canonical. The HTML version, when produced, mirrors the
markdown content without adding filler.

## Structure

### 1. Plan Overview (synthesis)

The orientation section, read first by any role or spawned subagent before it
acts on a part. It states:

- The product goal and what the active release delivers.
- How the work is organized and how the pieces fit together.
- The key architecture constraints and cross-cutting concerns.
- The active release and where the current work sits in the roadmap.
- A short map from work items to the user-visible capability each completes.

Keep this section current. When a subagent is spawned for one work item, this is
what lets it understand how its scope fits the overall picture.

### 2. Release Organization

The plan is architected around release content. Organize work by release:

- One section per release, current active release first.
- Each release records its goal or theme and the work items it contains.
- Subsequent releases add their own sections; earlier release sections remain
  for continuity and context.

| Release | Goal | Work items | Status |
| --- | --- | --- | --- |
| `<milestone>` | `<release goal>` | `<item ids>` | planned / active / shipped |

### 3. Work Item Breakdown

For each work item (an issue or a story, depending on tooling), record:

- ID and title, mapping to the GitHub Issue once it is created.
- Source reference: the roadmap feature, feature slice, or defect record.
- Scope: the capability this item completes.
- Out of scope: what it intentionally excludes.
- Success criteria: observable behavior that proves the item works.
- Definition of done: the completion checklist (see below).
- Dependencies: the items that must complete first.
- Parallelization: the track it belongs to and whether it can run concurrently.
- Architecture constraints and the expected test-first starting point.
- Roles involved, and selected theme and color mode for visual surfaces.

Work items must trace to product intent: every developer-executable item
completes a roadmap feature, a slice of one, or a defect fix. No orphan items.

### 4. Definition of Done

Every work item carries an explicit definition of done. Scale it to the effort:
match the rigor to the product's importance, scope, and size rather than
applying every gate to every item.

Always required, at every budget:

- TDD: focused failing test written first, minimal passing implementation,
  refactor with tests green — TDD evidence recorded.
- Success and acceptance criteria demonstrated.
- Evidence recorded on the issue.

Required when they apply to the effort and surface — fuller for `standard` and
`full`, lighter or omitted for small `inline` and `mini` work:

- CI green or a recorded waiver, when the repository has CI.
- Theme compliance, when the item changes a visual surface: tokens, states,
  visible focus, and color-mode behavior.
- Broader regression, QA depth, packaging, and operational checks, to the depth
  the assurance level and budget call for.

Do not inflate a small item's definition of done with gates its scope does not
warrant, and never drop TDD to save time.

### 5. Sequencing and Parallelization

- Order the work items by dependency, risk, and architectural priority.
- Identify tracks that can run in parallel to speed up delivery, and mark the
  integration points where parallel tracks converge.
- For `standard` and `full` work, include a Mermaid diagram showing the sequence
  and the parallel tracks.

Treat parallelization as a first-class opportunity during breakdown, not an
afterthought. Prefer independent slices that let multiple developers or
subagents progress at once, as long as dependencies and integration points stay
explicit. This is how the plan turns scope into faster delivery without losing
control.

### 6. Cross-Release Continuity

Record how the active release builds on shipped work and sets up planned work,
so the cumulative plan stays coherent as releases are added.

## Maintenance

The Technical Lead keeps the plan current as work items are created, sequenced,
completed, deferred, or added in later releases. The plan and the GitHub Issues
log must not contradict each other: issues carry execution state; the plan
carries the synthesis, sequencing, parallelization, and release organization.
