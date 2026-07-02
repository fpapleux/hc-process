# Wildwest Requirements

## Product Direction

Wildwest manages autonomous AI operators as real Linux technical accounts.

The goal is not to create ordinary human login users. The goal is to create non-human operating identities that can run persistent agent processes, own files/processes, receive scoped access, and be revoked or audited independently from the human who delegated work.

All product and architecture decisions must be recorded as Markdown files in `docs/decisions/`.

## Core Concepts

- **Agent**: a named autonomous operator managed by Wildwest.
- **Technical account**: a Linux user account representing one agent, typically named with a prefix such as `ww-`.
- **Agent service**: a persistent `systemd` service running as the agent's Unix user.
- **Runtime**: the underlying agent engine the service invokes, initially `codex` or `claude`.
- **Workspace**: a directory the agent can work inside.
- **Spool**: filesystem-based inbox/outbox used as the first communication channel.
- **Grant**: a Wildwest-managed access rule that lets a user, group, or agent access a path.
- **ACL**: Linux access control list used when multiple groups need different permissions on the same path.
- **Privileged action broker**: future Wildwest component that mediates narrow privileged actions instead of giving agents raw sudo.

## Initial Requirements

### User Creation

Wildwest must create each agent as a Linux technical account:

- create a Unix user and matching group
- use a predictable username, defaulting to `ww-<agent-name>`
- create a dedicated home directory under `/var/lib/wildwest/agents/<agent-name>`
- lock password login
- avoid granting broad groups or sudo by default
- assign ownership of the agent home and runtime directories to the agent user

The account may have a real shell for tool compatibility, but direct login is not the product interface.

### Agent Service

Wildwest must install a `systemd` unit per agent:

- service name: `wildwest-agent-<agent-name>.service`
- runs as the agent user and group
- starts automatically unless explicitly disabled
- restarts on failure
- uses the agent workspace as its working directory
- loads runtime configuration from `/etc/wildwest/agents/<agent-name>.env`

The service is the autonomy boundary: the agent can keep running and processing work without a human invoking each command.

### Runtime Selection

The first CLI must support a runtime configuration flag:

- `codex`
- `claude`

The CLI should not hardcode secrets or assume a global auth model. Authentication is handled by the installed runtime and environment available to the agent account.

### Communication Model

The first communication channel is a local filesystem spool:

- inbox: `/var/lib/wildwest/agents/<agent-name>/inbox`
- outbox: `/var/lib/wildwest/agents/<agent-name>/outbox`
- run state: `/var/lib/wildwest/agents/<agent-name>/run`
- logs: `/var/lib/wildwest/agents/<agent-name>/logs`

Submitting work means writing a prompt/job file into the inbox. The service picks it up, runs the configured runtime, and writes output artifacts into the outbox.

This is intentionally simpler than HTTP. It is local, inspectable, permissionable, easy to audit, and aligns with Linux ownership semantics.

### File Access Grants

Wildwest must make file grants understandable to ordinary operators.

Operator-facing access levels:

- `none`: no access
- `read`: inspect/list/read files
- `run`: read and execute/traverse, but not modify
- `write`: create/edit/delete/rename
- `admin`: manage files and access policy

Classic Unix ownership and groups are enough for simple cases. Linux ACLs are required when multiple users or groups need different rights on the same path.

Wildwest should eventually provide commands such as:

```bash
wildwest grant <agent-or-group> <path> --access run
wildwest grant <agent-or-group> <path> --access write
```

Internally, those commands can map to `setfacl` and default ACLs. Wildwest should apply both current ACLs and default directory ACLs so new files inherit the intended policy.

### CLI Commands

Initial command:

```bash
wildwest create-user <agent-name> --runtime codex|claude
```

Useful immediate follow-ups:

```bash
wildwest submit <agent-name> "prompt text"
wildwest status <agent-name>
wildwest logs <agent-name>
```

Only `create-user` is required for the first implementation.

## Security Requirements

- No password login for agent accounts.
- No unrestricted sudo for agent accounts.
- No shared human credentials by default.
- Agent service runs as the agent account, not as root.
- Root is used only for provisioning and `systemd` installation.
- Agent directories should default to private permissions.
- File access should be granted through narrow groups or ACLs instead of broad shared groups when possible.
- Privileged operations should eventually flow through a Wildwest action broker, not direct sudo.

## Open Design Questions

- Should direct SSH login be blocked through Wildwest-managed `sshd_config` or documented as an operator responsibility?
- Should runtime auth be copied/provisioned into the agent account, delegated through a broker, or configured manually?
- Should the spool format become structured JSON instead of plain prompt files?
- When should Wildwest add an HTTP or Unix-socket API?
- What exact privileged actions should v1 broker instead of exposing sudo?
- Should Wildwest store intended ACL grants and reconcile them over time?
