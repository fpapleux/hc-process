# 0004 Privileged Access Uses A Broker

## Decision

Wildwest should not give agents raw sudo as the default privileged access model.

The intended model is a privileged action broker: an agent requests a narrow action, Wildwest checks policy, logs the request, and performs the action if allowed.

## Reason

`sudo` can restrict exact commands, but it becomes brittle with dynamic arguments, paths, shells, package managers, editors, and escape hatches.

A broker lets Wildwest express granular capabilities such as restarting one service, changing ownership under one path, or installing approved packages with approval.

## Consequences

- Agent accounts are created without unrestricted sudo.
- Future privileged actions should be represented as named capabilities.
- The broker can initially use root-owned wrappers or sudoers internally, but that should not be the product abstraction.

## Open Questions

- What are the first privileged actions Wildwest should broker?
- Should the broker expose a Unix socket, CLI, or both?
- What approval model is required for high-risk actions?

