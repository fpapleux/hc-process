# TOOLS.md - Operations Lead

This role uses only the local tool manuals in `tools/`.

Read before acting:

- `tools/github-issues.md`
- `tools/local-validation.md`
- `tools/service-manager.md`
- `tools/secrets-tls.md`
- `tools/monitoring.md`

Do not use tool manuals that are not present in this folder.

## Authority Summary

GitHub Issues:

- Read operational issues.
- Create issues for operational findings.
- Comment with non-secret operational evidence.
- Edit labels for operational status when the process permits it.

Local validation:

- Run health, readiness, restart, status, logging, and smoke commands defined by
  the product runbooks.
- Record exact commands and results without exposing secret values.

Service manager:

- Inspect, start, stop, restart, and status-check services through documented
  environment runbooks.
- Configure service-manager files only when the approved runbook or issue gives
  Operations authority for that environment.

Secrets and TLS:

- Create, install, rotate, permission, and verify scoped host-managed
  credentials and TLS material outside Git.
- Record only secret names, locations, owners, and rotation procedures.

Monitoring:

- Inspect and maintain dashboards, alerts, and logs through approved tools.
- Record operational status and incidents.

## Forbidden Operations

- Do not write application code.
- Do not run QA acceptance tests.
- Do not promote releases.
- Do not create GitHub Releases or tags.
- Do not store secret values, TLS private keys, tokens, or passwords in Git,
  issue comments, release notes, logs, or chat.
- Do not use production credentials for non-production checks.

## Required Operations Evidence

For scoped provisioning or maintenance, record:

- Target environment.
- Related issue or runbook.
- Secret root and TLS file paths without values.
- Ownership and permissions applied.
- Service-manager status or documented setup.
- Health/restart/status checks run and results.
- Logs reviewed for correlation IDs and secret redaction when applicable.
- Handoff recipient and remaining blockers.
