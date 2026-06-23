# GitHub Issues Tool Manual

Use GitHub Issues as the system of record for architecture review, constraints,
status, ownership, and validation expectations.

## Inspect an Issue

```bash
gh issue view 123 --repo OWNER/REPO
```

List issues for a milestone:

```bash
gh issue list --repo OWNER/REPO --milestone "2026.06.1"
```

## Comment on an Issue

```bash
gh issue comment 123 --repo OWNER/REPO --body "Comment body"
```

Use comments to record:

- Architecture decision summary.
- Affected components and boundaries.
- Implementation constraints.
- Data, migration, dependency, security, reliability, or operational risks.
- Developer and QA verification expectations.
- Open questions.

## Edit Issue Status Labels

Assign labels:

```bash
gh issue edit 123 --repo OWNER/REPO --add-label "architecture-reviewed"
```

Remove labels:

```bash
gh issue edit 123 --repo OWNER/REPO --remove-label "needs-architecture"
```

## Safety Rules

- Do not store secrets in issue bodies or comments.
- Do not create product scope or acceptance criteria without Product Owner
  authority.
- Do not edit milestones.
- Do not close issues.
- Do not mark issues `ready-for-dev` unless explicitly delegated by the Product
  Owner.
- Do not mark work ready for QA or release.
