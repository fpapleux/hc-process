# 0002 Agent Accounts Are Technical Accounts

## Decision

Wildwest agent users are Linux technical accounts, not normal human user accounts.

The default username format is `ww-<agent-name>`.

## Reason

Agents need first-class operating identities, but they are not humans. They should not inherit human credentials, login paths, broad groups, or personal shell state.

Technical accounts allow Wildwest to assign ownership, permissions, process identity, home/state directories, and audit records independently from the delegating human.

## Consequences

- Agent accounts get dedicated homes under `/var/lib/wildwest/agents/<agent-name>`.
- Password login is locked.
- Broad sudo or human-equivalent group membership is not granted by default.
- A real shell may still be configured for tool compatibility.

## Open Questions

- Should Wildwest actively block SSH login through managed SSH/PAM policy?
- Should the default shell remain `/bin/bash`, or should it become configurable per runtime?

