# Local Validation Tool Manual

Use local validation commands defined by product operations runbooks.

## Evidence Format

Record exact commands and results:

```text
Verification:
- <command>: passed
- <command>: failed, reason: <summary>
- <manual check>: not run, reason: <reason>
```

## Required Behavior

- Run health, readiness, restart, status, log, monitoring, or smoke commands
  defined by the product runbooks.
- Record commands exactly as executed.
- Record failures plainly.
- If a relevant command is unavailable, record that it was unavailable and why.
- Do not claim operational readiness from code inspection alone.

## Safety Rules

- Do not run destructive commands unless the role process and runbook permit
  them.
- Do not use production credentials for non-production validation.
- Do not mutate production data during local validation.
- Do not print secret values or TLS private keys.
