# Service Manager Tool Manual

Use the host service manager only through approved product runbooks or
operator-authorized operational procedures.

## Inspect Services

```bash
systemctl status SERVICE --no-pager
systemctl --user status SERVICE --no-pager
```

## Restart Services

```bash
systemctl restart SERVICE
systemctl --user restart SERVICE
```

## Validate Units

```bash
systemd-analyze verify PATH/TO/unit.service
```

## Safety Rules

- Confirm the target environment before starting, stopping, or restarting a
  service.
- Do not modify production service units without an approved runbook or
  operator authorization.
- Do not leave services in a failed or half-configured state without recording
  the blocker and rollback or recovery action.
