# 0007 Initial CLI Starts With Create User

## Decision

The first executable command is:

```bash
./wildwest create-user <agent-name> --runtime codex|claude
```

## Reason

Creating the technical account and service is the foundation for the rest of Wildwest. It proves the core primitive before adding policy, brokered privileges, HTTP, or a richer job model.

## Consequences

- The CLI starts as a local Python executable named `wildwest`.
- It provisions Linux users, directories, config, runtime script, and systemd units.
- It includes `--dry-run` so the generated operations can be inspected before root execution.

## Open Questions

- Should the command eventually be renamed `create-agent` while keeping `create-user` as an internal primitive?
- Should the CLI be rewritten in Rust or Go once behavior stabilizes?

