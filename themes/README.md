# Themes

Themes define visual identity for Human-Codex outputs.

A theme is the reusable design layer for anything with a web surface:

- single-page HTML product briefs, UX documents, architecture documents, and
  operations plans;
- API platforms and developer portals;
- dashboards, admin tools, and internal apps;
- product UIs generated through the process.

The theme controls look and feel. It does not define product scope, role
authority, implementation architecture, or release contents.

## Process Role

The process has four reusable dimensions:

```text
Role controls authority.
Product controls intent.
Technology controls technique.
Theme controls visual identity.
```

Themes are selected by the operator, Product Owner, or UX Design Lead depending
on the stage of work:

- Product Owner may record a preferred theme as product intent.
- UX Design Lead confirms the theme for screen, document, API, report, and
  dashboard surfaces.
- Architect records theme integration constraints only when the selected theme
  affects technical decisions.
- Technical Lead carries the selected theme into executable issues.
- Developer implements against the selected theme's token and component
  contract.
- QA validates visible surfaces against the selected theme and accessibility
  expectations.
- Release includes theme assets and provenance in release evidence when theme
  material ships.

## Theme Contract

Every theme must include:

```text
themes/<theme-id>/
  theme.yml
  README.md
  tokens/
  components/
  layouts/
  examples/
  upstream/
```

### `theme.yml`

Machine-readable theme metadata. It records:

- stable theme id and display name;
- license and source/provenance;
- upstream repository and pinned commit when derived from another project;
- supported surfaces;
- supported color modes;
- token files and component bundles;
- maturity status.

### `tokens/`

Design tokens are the source of visual truth. A token set should define:

- brand accents;
- text colors;
- surfaces and backgrounds;
- borders;
- typography;
- spacing;
- radii;
- shadows;
- state colors;
- focus rings;
- motion durations;
- z-index layers where needed.

CSS custom properties are the default token transport because they work for
static HTML documents, framework apps, and generated prototypes.

### `components/`

Component guidance and CSS for reusable UI primitives:

- buttons;
- inputs and forms;
- tables;
- cards;
- navigation;
- tabs;
- badges and pills;
- feedback and alerts;
- modals and overlays;
- loading and empty states;
- accessibility helpers.

Components consume tokens. They should not hard-code brand colors, shadows, or
typography when a token exists.

### `layouts/`

Surface-level shells:

- `document`: single-page HTML docs and decks;
- `app-shell`: authenticated app UI;
- `api-platform`: API docs, keys, playgrounds, and endpoint reference;
- `dashboard`: metrics, tables, cards, and operational views;
- `marketing-lite`: simple public or onboarding surfaces when needed.

Layouts define composition, navigation, density, and responsive behavior.

### `examples/`

Reference HTML or screenshots showing correct theme usage. Agents should inspect
examples before creating a new visual surface.

### `upstream/`

For copied or derived themes, this folder records:

- source repository;
- source commit;
- copied directories or files;
- import date;
- local modifications;
- license and notice requirements.

Actual copied upstream files may live here or in `tokens/`, `components/`, and
`layouts/` depending on how usable they are in the Human-Codex process. The
provenance record must stay clear either way.

## Theme Selection

When a product or artifact has a visual surface, record:

```text
Theme:
Theme source:
Theme mode: light | dark | both | system
Theme surfaces:
Theme deviations:
```

For small work, this can be a single issue note. For `standard` and `full` work,
include it in the product brief and UX design document.

## Default Theme

Until a product records a different theme, use:

```text
Theme: leantime-inspired
Theme mode: both
```

The current default theme is intentionally app-like: rounded surfaces, compact
work-management density, strong blue/teal accents, light/dark support, and
accessibility-aware focus and typography.

## Applying A Theme

For static HTML documents, load the selected theme in this order:

```html
<link rel="stylesheet" href="themes/leantime-inspired/tokens/light.css">
<link rel="stylesheet" href="themes/leantime-inspired/tokens/dark.css">
<link rel="stylesheet" href="themes/leantime-inspired/components/base.css">
<link rel="stylesheet" href="themes/leantime-inspired/layouts/document.css">
```

Set the root attributes:

```html
<html data-theme="leantime-inspired" data-color-mode="system">
```

For applications, the technology profile can translate the same token contract
into framework styles, but the source of visual truth remains the selected
theme.

## Theme Compliance

A visual surface is theme-compliant when:

- it loads the selected theme tokens;
- it uses the theme's component classes or equivalent token-backed styles;
- light/dark behavior matches the selected mode policy;
- visible state, errors, focus, and disabled behavior use theme state tokens;
- typography and spacing match the theme scale;
- copied or derived theme material preserves provenance and license notices.

QA records theme issues as visual defects unless the Product Owner explicitly
accepts the deviation.
