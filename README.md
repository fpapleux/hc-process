# Human-Codex Process

This repository defines a practical operating system for agent-managed product
work. It is designed for teams that want AI agents to do real delivery work
without collapsing product ownership, engineering judgment, QA, release, and
operations into one unbounded assistant.

The core idea is simple:

```text
Role controls authority.
Product controls intent.
Technology controls technique.
Theme controls visual identity.
```

That separation is what makes the process useful. A Developer agent can write
code, but it cannot decide what belongs in the release. A Product Owner agent
can define acceptance criteria, but it cannot prescribe implementation. QA can
validate behavior, but it does not fix defects in the developer lane. Release
can promote approved work, but only after QA and manifest gates are satisfied.

The result is a process that gives agents enough autonomy to move quickly while
keeping the things humans care about visible: scope, approval, evidence,
environment safety, and production risk.

## Why This Exists

Most agent workflows start with a prompt and end with a diff. That is fine for
small edits, but it breaks down when the work needs product judgment, design
constraints, architecture decisions, test evidence, release coordination, or
operational ownership.

This process turns agent work into a delivery system:

- Product intent is captured before implementation starts.
- UX, architecture, and technical planning scale to the risk of the work.
- Development is driven by TDD, with failing-test and passing-rerun evidence.
- QA validates pushed branches independently from developer workspaces.
- Release promotes only tested and approved commits.
- Production and non-production runtime worlds stay physically separated.
- Every handoff records evidence instead of relying on chat memory.

It is intentionally procedural, but not heavy by default. A tiny defect can use
inline issue notes. A personal tool can use a compact brief and scoped
architecture. A production service can require the full chain: brief, UX,
architecture, development plan, QA evidence, release manifest, staged
deployment, and operations readiness.

## The Operating Model

Work is divided across four dimensions.

### Role

The role defines authority: what an agent is allowed to decide, edit, validate,
or promote. Role boundaries prevent a single agent from silently changing
scope, implementation, validation, and release state in one pass.

### Product

The product profile defines intent: vision, roadmap, domain language,
acceptance criteria, repositories, deployment targets, and accumulated product
memory. Product context does not grant permissions by itself.

### Technology

Technology profiles define technique: test commands, lint commands, packaging
rules, style conventions, security practices, and stack-specific failure modes.
Technology guidance tells an authorized role how to work correctly; it does not
authorize that role to cross process boundaries.

### Theme

Theme profiles define visual identity: tokens, typography, color modes,
component styling, layout shells, examples, and provenance. Theme guidance
tells a visual surface what it should feel like across single-page HTML docs,
API platforms, dashboards, internal tools, and product UIs. It does not decide
product scope, architecture, or implementation technology.

A concrete agent profile is assembled from these pieces:

```text
role profile
+ product profile
+ one or more technology profiles
+ theme profile when the work has a visual surface
```

Example:

```text
agent: product-python-developer
role: agents/developer
product: products/product-name
technology:
  - technologies/python
theme:
  - themes/leantime-inspired
```

This keeps permissions, business intent, engineering practice, and visual
identity independent enough to reason about.

## Lifecycle

The default lifecycle is:

1. Product Owner defines what to build, why it matters, scope boundaries, and
   acceptance criteria.
2. UX Design Lead defines the structural experience: flows, commands, screens,
   states, accessibility, and design-to-architecture implications.
3. Architect records technical direction, boundaries, constraints, risks, and
   verification expectations where architecture review is needed.
4. Technical Lead turns accepted product, UX, and architecture inputs into
   executable GitHub Issues, sequences work, prepares environments, and marks
   only process-ready issues `ready-for-dev`.
5. Developer implements one `ready-for-dev` issue at a time in `dev/**` using
   TDD: focused failing test first, minimal passing implementation second,
   refactor only while tests stay green. For visible surfaces, the Developer
   implements through the selected theme's tokens and components.
6. QA validates the pushed branch in `tst/**`, checks CI and TDD evidence, runs
   acceptance, regression, accessibility, and theme-compliance checks, and
   records pass/fail evidence.
7. Release assembles only QA-passed work into `rel/**`, runs release-level QA,
   promotes validated source, deploys through approved gates, and tags only
   after production promotion.
8. Operations Lead owns production runtime health after release: monitoring,
   incidents, production credentials, TLS material, runbooks, and operations
   plan maintenance.

The lifecycle is not meant to add ceremony to every task. It scales by artifact
budget:

- `inline`: defect, tiny script, or issue-local change.
- `mini`: prototype or personal tool with narrow scope.
- `standard`: feature or product needing downstream UX or architecture.
- `full`: production, enterprise, regulated, or multi-actor product.

The process asks for the smallest artifact set that still protects the work.

## What Makes It Different

### It Is Built For Agent Authority, Not Just Agent Output

The process assumes agents can take real action. That makes authority boundaries
non-negotiable. Each role has a lane, a tool surface, and explicit stop
conditions. Agents can move fast inside their lane, but handoffs require
evidence.

### It Makes The Filesystem Match The Process

Product folders use phase-specific checkouts:

```text
product/
  dev/   active development checkouts
  tst/   QA and release-test checkouts
  rel/   local release source checkouts
  architecture/
  agents/
  process/
  README.md
```

Developers work in `dev/**`. QA validates in `tst/**`. Release operates in
`rel/**`. Promotion between phases is explicit and owned by the next role.

### It Treats TDD As Delivery Discipline

Developer work is test-driven. For every feature, defect fix, refactor, or
behavior change, the Developer records:

- the focused test added or updated;
- the expected failing run before production code changes;
- the minimal implementation;
- the passing focused rerun;
- broader relevant checks before QA handoff.

Technical Lead enforces TDD expectations before work starts. QA checks TDD
evidence before marking work `qa-passed`. Release checks that included issues
carry Developer TDD evidence or an explicit Technical Lead exception.

### It Gives Every Web Surface A Reusable Visual Identity

The process now treats visual design as reusable infrastructure. A theme
records the look and feel once, then every HTML brief, architecture document,
UX document, API platform, dashboard, internal tool, and app UI can consume the
same token and component contract.

The default theme is `leantime-inspired`: an app-like visual identity derived
from Leantime's public theme implementation. It brings rounded operational
surfaces, blue/teal accents, compact work-management density, light/dark mode,
soft shadows, readable forms and tables, and accessible focus behavior into
Human-Codex outputs.

Themes are designed to be swappable. A product can start with
`leantime-inspired`, then later select another theme without rewriting the
role process, product brief structure, technology standards, or release
workflow.

### It Separates Runtime Worlds

Production and non-production are separate runtime worlds. A product, tool,
service, worker, scheduler, CLI daemon, UI, API, or mutable runtime must not be
able to read, mutate, select, proxy, or manage both production and
non-production.

That separation must be enforced by real controls: service users, scoped
credentials, filesystem permissions, network bindings, databases, queues,
buckets, config roots, and runtime directories. Labels, route params, flags,
and operator discipline are not controls.

### It Preserves Human Decision Points

The process keeps human judgment where it belongs:

- Product scope and release contents are Product Owner decisions.
- Operator approval is required for defined handoff gates.
- Major production release decisions are not inferred by agents.
- Waivers are explicit records with scope, reason, approver, risk owner,
  expiry, and follow-up.

This lets agents execute without turning ambiguity into silent policy.

## Repository Structure

```text
.
  agents/        reusable role profiles and profile composition rules
  documents/     document specifications for briefs, UX, architecture, ops
  process/       operating process definitions and lifecycle rules
  security/      environment, secrets, and security follow-up notes
  technologies/  technology-specific engineering standards
  themes/        reusable visual identity themes for web surfaces
  tools/         canonical reusable tool manuals
  wildwest/      example active product using this process
  README.md      repository overview
```

## Agents

[agents/](agents/README.md) defines reusable role profiles.

Current role profiles:

- Product Owner
- UX Design Lead
- Architect
- Technical Lead
- Developer
- QA
- Release
- Operations Lead

Each role profile defines identity, authority, boundaries, tool access,
handoffs, recurring checks, and interaction style. The profile files are
deliberately repetitive in the places that matter because agents often enter
through different surfaces. The canonical process remains in `process/**`, and
the role profiles mirror the rules an agent must remember at runtime.

## Process

[process/](process/overview.md) defines how work moves from idea to production.

Current process definitions:

- [Overview](process/overview.md)
- [Product Owner](process/product-owner.md)
- [UX Design Lead](process/ux-design-lead.md)
- [Architect](process/architect.md)
- [Technical Lead](process/tech-lead.md)
- [Developer](process/developer.md)
- [QA](process/qa.md)
- [Release](process/release.md)
- [Operations Lead](process/operations-lead.md)
- [Product environments](process/environments.md)

The overview is the starting point. Role files define the authority,
responsibilities, workflow, evidence requirements, stop conditions, and handoff
rules for each stage.

## Documents

[documents/](documents/) defines the artifacts that carry intent and evidence
between roles.

Current document types:

- [Product Brief](documents/product-brief.md): product vision, goals, scope,
  acceptance criteria, roadmap, and release intent.
- [UX Design Document](documents/ux-design-document.md): user flows,
  interaction states, wireframes, accessibility, and design-to-architecture
  bridge.
- [Architecture Design Document](documents/architecture-design-document.md):
  system boundaries, components, contracts, risks, test-first expectations,
  and technical decisions.
- [Operations Plan](documents/operations-plan.md): deployment, monitoring,
  credentials, runbooks, readiness, and operational procedures.

Documents scale with the artifact budget. The process prefers a compact,
accurate issue note over a large template filled with filler.

## Technologies

[technologies/](technologies/README.md) defines stack-specific practices.

Current profile:

- [Python](technologies/python/README.md)

Technology profiles define how authorized agents should work in a stack: test
strategy, TDD order, validation commands, packaging, style, security, and common
failure modes. They are reusable across products.

## Themes

[themes/](themes/README.md) defines visual identity themes.

Current default theme:

- [Leantime Inspired](themes/leantime-inspired/README.md)

Themes contain design tokens, component guidance, layout shells, examples, and
upstream provenance. They are used whenever the process produces a visual web
surface: static HTML docs, API platforms, dashboards, internal tools, product
UIs, and browser-viewable process artifacts.

The selected theme is recorded in the product brief or UX design document for
`standard` and `full` work, or in the issue notes for smaller visual changes.
Developer implementation uses theme tokens before hard-coded visual values. QA
records visible theme drift as a defect unless the Product Owner accepts the
deviation.

## Tools

[tools/](tools/README.md) contains canonical reusable tool manuals.

Tool documentation uses two layers:

- Canonical `tools/*.md` files explain how a tool works.
- Role `TOOLS.md` files grant permission and point only to the manuals that
  role can use.

This prevents tool knowledge from becoming tool authority.

## Security And Environments

[security/](security/README.md) and [process/environments.md](process/environments.md)
define environment and secret-handling expectations.

The most important rule is that production and non-production separation is not
waivable. Release evidence cannot rely on production credentials in
non-production, dummy TLS material, disabled certificate verification, browser
security exceptions, or fixture secrets masquerading as operational readiness.

## How To Use This Process

Start with the outcome you need and invoke the role that owns the next decision.

- Need scope, acceptance criteria, or release inclusion? Start with Product
  Owner.
- Need flows, CLI shape, screen behavior, or accessibility? Use UX Design Lead.
- Need boundaries, dependencies, migrations, or security direction? Use
  Architect.
- Need issues sequenced and made ready for implementation? Use Technical Lead.
- Need code written for a ready issue? Use Developer.
- Need independent validation? Use QA.
- Need release assembly, promotion, and deployment evidence? Use Release.
- Need production runtime material, monitoring, or incidents handled? Use
  Operations Lead.

The process works best when every issue can answer four questions before code
starts:

1. What product outcome or defect record does this issue serve?
2. What observable acceptance criteria define done?
3. What technical constraints or architecture notes apply?
4. What focused test should fail first?

For visual work, add a fifth:

5. What theme, mode, and surface contract should the UI follow?

If those answers are available, agents can work with far less ambiguity and far
more useful autonomy.

## Adoption Notes

You do not need to adopt the entire system on day one. The useful starting set
is:

1. Use role boundaries from `agents/**`.
2. Use the lifecycle and labels from `process/overview.md`.
3. Require TDD evidence from Developer.
4. Require QA to validate pushed branches, not developer workspaces.
5. Keep release promotion separate from development.
6. Enforce production/non-production runtime separation from the beginning.
7. Select a theme for every visual surface and implement through its tokens.

From there, add artifact depth as the product risk justifies it.

The goal is not bureaucracy. The goal is reliable agent delivery: clear intent,
bounded authority, evidence-rich handoffs, and production changes that can be
explained after the fact.
