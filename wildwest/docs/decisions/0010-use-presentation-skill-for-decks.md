# 0010 Use Presentation Skill For Decks

## Decision

Install and use the global Codex skill `presentation` for Wildwest presentation decks.

The skill is installed at:

```text
~/.codex/skills/presentation
```

## Reason

Wildwest is still in founder/product exploration, and presentation decks are likely to be useful for explaining the thesis, architecture, and operating model.

Using a reusable skill keeps deck output consistent: fixed sidebar/topbar layout, embedded DM fonts, keyboard navigation, reusable components, and Mermaid diagram support.

## Consequences

- Future presentation work can invoke `$presentation`.
- Decks should default to standalone browser-openable HTML files.
- Generated decks should use the bundled fonts and design system unless a different direction is explicitly requested.

## Open Questions

- Should Wildwest keep generated decks in a dedicated `presentations/` folder?
- Should founder/investor decks and operator/technical decks use separate templates?

