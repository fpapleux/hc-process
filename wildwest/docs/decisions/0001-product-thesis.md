# 0001 Product Thesis

## Decision

Wildwest is a control layer for autonomous AI operators on real systems.

It should not be framed only as a CLI wrapper around Linux users. The larger product thesis is agent control infrastructure: identity, access, execution boundaries, policy, logging, lifecycle, and audit for AI agents.

## Reason

AI agents are becoming operational actors. Serious teams, especially regulated teams, need to know which agent acted, on whose authority, under which policy, with which tools, and with what resulting changes.

Linux user accounts are the first concrete wedge because they provide a proven operating-system identity primitive.

## Consequences

- Wildwest should preserve a clear line between agent capability and agent governance.
- Wildwest does not need to build its own foundation model or agent loop first.
- The durable product value is the boundary and control layer around agents.

## Open Questions

- Is the first commercial audience developers, security teams, regulated enterprises, or AI platform companies?
- Which compliance mappings should be prioritized first?

