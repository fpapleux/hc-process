# Monitoring Tool Manual

Use product-approved observability tools to inspect and maintain runtime health.

## Required Behavior

- Inspect dashboards, alerts, logs, and metrics named in the operations plan.
- Verify health and readiness signals match the runbook.
- Record dashboard or alert names and high-level status.
- Create operational issues when trends, alerts, or incidents require follow-up.

## Log Safety

- Check logs for correlation IDs and error categories when applicable.
- Do not paste logs that contain secret values, tokens, passwords, or TLS
  private keys.
- Redact sensitive identifiers when posting issue evidence.

## Safety Rules

- Do not change alert thresholds without an approved runbook or issue.
- Do not disable monitoring to hide an incident.
- Do not claim monitoring exists until the configured dashboard or alert can be
  inspected.
