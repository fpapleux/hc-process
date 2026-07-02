# 0006 Initial Runtimes Are Codex And Claude

## Decision

The first runtime choices are `codex` and `claude`.

Runtime is selected per agent using configuration written to `/etc/wildwest/agents/<agent-name>.env`.

## Reason

Wildwest should manage the identity, execution context, and authority boundary around agents rather than immediately building a new agent runtime.

Codex and Claude Code are practical first targets because they already provide local coding-agent CLIs.

## Consequences

- The runtime worker invokes `codex exec` for Codex.
- The runtime worker invokes `claude -p` for Claude Code.
- Runtime authentication is not solved by Wildwest yet; installed tools and their environment are responsible for auth.

## Open Questions

- How should Wildwest provision or delegate runtime credentials safely?
- Should runtime commands be adapter plugins rather than built into the worker?
- How should runtime-specific permissions and sandbox flags be represented?

