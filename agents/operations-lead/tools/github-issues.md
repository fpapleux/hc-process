# GitHub Issues Tool Manual

Use GitHub Issues as the system of record for operational findings,
provisioning evidence, blockers, and handoffs.

## Inspect an Issue

```bash
gh issue view 123 --repo OWNER/REPO
```

## Create an Operational Issue

```bash
gh issue create \
  --repo OWNER/REPO \
  --title "Short operational issue title" \
  --body "Issue body"
```

Issue bodies must include the target environment, observed problem or required
provisioning work, impact, evidence, and expected handoff.

## Comment on an Issue

```bash
gh issue comment 123 --repo OWNER/REPO --body "Comment body"
```

Use comments to record:

- Target environment.
- Secret and TLS file paths without values.
- Service-manager status.
- Health, readiness, restart, and monitoring checks.
- Blockers and handoffs.

## Edit Issue Metadata

Assign labels:

```bash
gh issue edit 123 --repo OWNER/REPO --add-label "ops-identified"
```

Remove labels:

```bash
gh issue edit 123 --repo OWNER/REPO --remove-label "status:blocked"
```

## Safety Rules

- Do not store secret values in issue bodies or comments.
- Do not close release issues unless the role process grants that authority.
- Do not edit milestones unless the role process grants that authority.
- Do not mark release validation complete; hand off to Release for that.
