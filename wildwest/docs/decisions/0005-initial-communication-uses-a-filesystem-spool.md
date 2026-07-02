# 0005 Initial Communication Uses A Filesystem Spool

## Decision

The initial agent communication model is a local filesystem spool.

Each agent has:

- `inbox` for prompt/job files
- `outbox` for results
- `run` for in-flight state
- `logs` for local logs

## Reason

A filesystem spool is local, inspectable, permissionable, and easy to reason about with standard Linux ownership and group rules.

It avoids introducing HTTP authentication, ports, request signing, network exposure, or a long-running root daemon before those choices are necessary.

## Consequences

- The first runtime polls `inbox/*.prompt`.
- Results are written to a per-job directory under `outbox`.
- HTTP is deferred until there is a clear control-plane requirement.

## Open Questions

- Should job files remain plain text prompts or become JSON envelopes?
- Should the spool directory be writable by humans directly, by Wildwest CLI only, or by a group?
- Should later IPC use HTTP, a Unix domain socket, D-Bus, or gRPC over UDS?

