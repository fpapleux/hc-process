# Tokens

This folder holds the normalized Human-Codex token files for the
`leantime-inspired` theme.

Current files:

```text
light.css
dark.css
tokens.json
```

## Token Policy

- Keep light and dark mode parity.
- Use CSS custom properties as the primary transport.
- Preserve Leantime aliases during migration when they reduce adaptation cost.
- Prefer `--hc-*` names for stable Human-Codex tokens.
- Do not hard-code component colors when a token exists.
- Keep upstream Leantime variable aliases such as `--accent1`,
  `--secondary-background`, and `--box-radius` while the portable component
  layer is still maturing.
