# 0003 Agents Run As Autonomous Services

## Decision

Wildwest agents should run as persistent services, not only as commands invoked manually by a human.

Each agent gets a `systemd` service named `wildwest-agent-<agent-name>.service`.

## Reason

Manual command execution through `wildwest run agent -- command` is delegated execution, not autonomy.

For the Wildwest model, an agent should be able to keep running as its own Unix user and process work within its assigned permissions.

## Consequences

- The first CLI provisions a systemd unit per agent.
- The service runs as the agent user and group.
- The service can restart on failure.
- Human users communicate with the service rather than logging in as the agent.

## Open Questions

- Should Wildwest later support transient one-shot agents in addition to persistent services?
- Should services be supervised only by systemd, or should Wildwest add its own daemon?

