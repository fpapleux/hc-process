# Wildwest

Wildwest is an experimental control layer for autonomous AI operators on Linux.

The first implementation creates each agent as a Linux technical account and starts a persistent `systemd` service as that account. The service can invoke either Codex or Claude Code based on configuration.

## Current CLI

```bash
./wildwest create-user scout --runtime codex --dry-run
sudo ./wildwest create-user scout --runtime codex
sudo ./wildwest create-user editor --runtime claude --workdir /srv/project
```

The command creates:

- Unix user: `ww-<agent-name>`
- Home: `/var/lib/wildwest/agents/<agent-name>`
- Config: `/etc/wildwest/agents/<agent-name>.env`
- Service: `wildwest-agent-<agent-name>.service`
- Runtime: `/usr/local/lib/wildwest/wildwest-agent-runtime`

## Communication Model

The first communication path is a filesystem spool:

```text
/var/lib/wildwest/agents/<agent>/inbox   prompts/jobs
/var/lib/wildwest/agents/<agent>/outbox  results
/var/lib/wildwest/agents/<agent>/run     in-flight state
/var/lib/wildwest/agents/<agent>/logs    local logs
```

This is intentionally not HTTP yet. A spool is easier to reason about for local Linux security: ownership, group permissions, ACLs, `systemd`, and audit tooling all work naturally.

HTTP or a Unix socket can be added later as a broker/control-plane layer.

## Access Model

Wildwest should present file permissions as simple grants:

```text
read   inspect files
run    execute/read without modifying
write  modify files
admin  manage files and access policy
```

Under the hood, Wildwest will use normal Linux users, groups, and ACLs. See [docs/operator/access-model.md](docs/operator/access-model.md).

## Inspecting an Agent

```bash
systemctl status wildwest-agent-scout.service
journalctl -u wildwest-agent-scout.service -f
```

## Requirements

See [REQUIREMENTS.md](REQUIREMENTS.md).

## Decisions

Every product or architecture decision should be captured in `docs/decisions/`.

Start with [docs/README.md](docs/README.md).
