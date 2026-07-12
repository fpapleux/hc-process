# Leantime Inspired Theme

`leantime-inspired` is the default Human-Codex visual identity theme.

It is derived from Leantime's publicly available UI/theme implementation and is
intended to be copied, adapted, and normalized into a process-wide theme system.
The goal is not a pixel-perfect clone of Leantime. The goal is to keep the
parts that make the design useful across our tools:

- token-driven colors and surfaces;
- light and dark mode parity;
- blue/teal primary accent system;
- rounded operational UI;
- soft shadows and layered surfaces;
- compact app density;
- accessible focus behavior;
- alternate readable font options;
- reusable component classes.

## Upstream Source

Initial research source:

```text
Repository: https://github.com/Leantime/leantime
Commit: 4bd9f644785f3656a2caee7bd5e1edac7ab1e19c
Branch: master
```

The upstream project's theme model is based on:

- `public/theme/<theme>/theme.ini`
- `public/theme/<theme>/css/light.css`
- `public/theme/<theme>/css/dark.css`
- a runtime theme service that resolves active theme, color mode, color scheme,
  font, logo, background, and CSS/JS asset URLs;
- component CSS that consumes CSS custom properties;
- Blade components and layouts that compose the app shell.

## Adaptation Strategy

The Human-Codex theme should not stay coupled to Leantime's PHP/Blade runtime.
Instead, adapt the useful design layers into a portable contract:

1. Preserve upstream token files as provenance.
2. Normalize tokens into a stable Human-Codex token set.
3. Keep light and dark token files.
4. Create framework-neutral component CSS.
5. Create layout shells for docs, apps, API platforms, and dashboards.
6. Provide examples that agents can inspect before generating UI.

## Starter Files

Use these files for the first portable implementation:

```text
tokens/light.css
tokens/dark.css
tokens/tokens.json
components/base.css
layouts/document.css
```

Use upstream files in `upstream/` as source material and provenance, not as the
default runtime import path for new Human-Codex surfaces.

## Visual Direction

The theme should feel like a practical work platform:

- clear, compact, and operational;
- soft but not decorative;
- structured around app shells, cards, tables, forms, and boards;
- high confidence rather than high gloss;
- friendly enough for daily use without becoming whimsical.

## Token Categories

The normalized token contract should include:

- `--hc-accent-1`, `--hc-accent-2`
- `--hc-bg-page`, `--hc-bg-surface`, `--hc-bg-elevated`
- `--hc-text-primary`, `--hc-text-secondary`, `--hc-text-muted`
- `--hc-border-default`, `--hc-border-hover`
- `--hc-radius-sm`, `--hc-radius-md`, `--hc-radius-lg`
- `--hc-shadow-sm`, `--hc-shadow-md`, `--hc-shadow-lg`
- `--hc-font-sans`, `--hc-font-mono`
- `--hc-font-size-sm`, `--hc-font-size-base`, `--hc-font-size-lg`
- `--hc-focus-ring`
- `--hc-state-success`, `--hc-state-warning`, `--hc-state-danger`,
  `--hc-state-info`

Upstream Leantime variable names can be preserved as aliases where useful:

- `--accent1`
- `--accent2`
- `--primary-font-family`
- `--main-bg`
- `--secondary-background`
- `--primary-font-color`
- `--box-radius`
- `--element-radius`

## Surfaces

### HTML Document

Single-page briefs, architecture docs, UX docs, and operations plans should use
the theme's document layout. This replaces the current hard dependency on the
`$presentation` visual system as the only opinionated look.

### App Shell

Authenticated tools should use the app-shell layout: header, left navigation,
main content panel, responsive menu behavior, and token-backed content cards.

### API Platform

API platforms should use the same tokens with an API-specific shell: endpoint
navigation, request/response examples, auth guidance, playground panels, and
status feedback.

### Dashboard

Dashboards should use compact cards, tables, charts, filters, alerts, and
activity surfaces backed by the same token contract.

## Import Policy

Copied upstream files should be imported with source paths recorded in
`upstream/import-log.md`. Local adaptations should be described as changes, not
as original authorship.

When upstream code is too framework-specific, extract the design intent into
portable CSS and document the source file that informed it.
