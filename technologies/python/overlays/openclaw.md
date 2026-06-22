# OpenClaw Python Overlay

Use this overlay only when the role and product profile explicitly authorize
OpenClaw-specific Python work.

Generic Python code must not assume OpenClaw runtime state, global skills, or
access to operator environment files.

## Future Operating Rule

OpenClaw Python scripts receive only the configuration and skills assigned to
their role, product, and technology profile.

Do not read the operator vault directly from ordinary agent code.

## Environment Access

The historical pattern loaded:

```text
$OPENCLAW_STATE_DIR/environment.local
```

That pattern is being retired as a general agent convention.

Future scripts use role-scoped or product-scoped configuration prepared by the
operator or by approved tooling. The script validates the required values at
startup and fails before side effects when they are missing.

Acceptable future sources:

- role-scoped environment file;
- product-scoped environment file;
- approved MCP server;
- approved secret provider;
- explicit non-secret repo configuration.

Forbidden assumptions:

- The agent can read the operator vault.
- The agent can discover or load arbitrary shared skills.
- The agent can access every credential in `environment.local`.
- The agent can select production by passing `--prod`.

## Skills

Agents use only skills assigned to their concrete profile.

- Do not install skills.
- Do not load unrelated skills.
- Do not call skills outside role, product, or technology boundaries.
- If a needed skill is missing, stop and ask for the profile to be updated.

## Script Shape

For OpenClaw-managed scripts:

- Keep deterministic operations in scripts.
- Keep reusable domain logic importable.
- Put CLI parsing at the boundary.
- Load only approved scoped configuration.
- Validate target environment before side effects.
- Print machine-readable output to stdout when consumed by agents.
- Print diagnostics to stderr.

## Production Access

Developer and QA Python scripts do not perform production mutations.

Production mutation belongs behind approved tools operated by the role that owns
the production action. Scripts that can mutate production require:

- explicit role authority;
- scoped credentials;
- target environment supplied by process, not casual CLI flags;
- validation before side effects;
- audit evidence after execution.
