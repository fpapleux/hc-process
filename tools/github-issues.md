# GitHub Issues Tool Manual

Use GitHub Issues as the system of record for features, defects, acceptance
criteria, status, ownership, and validation evidence.

## Inspect an Issue

```bash
gh issue view 123 --repo OWNER/REPO
```

List issues for a milestone:

```bash
gh issue list --repo OWNER/REPO --milestone "2026.06.1"
```

## Create an Issue

```bash
gh issue create \
  --repo OWNER/REPO \
  --title "Short issue title" \
  --body "Issue body"
```

Issue bodies must include the fields required by the role process.

## Comment on an Issue

```bash
gh issue comment 123 --repo OWNER/REPO --body "Comment body"
```

Use comments to record:

- Branch names.
- Commit SHAs.
- Verification commands and results.
- QA evidence.
- Defect reproduction steps.
- Fix notes.

## Edit Issue Metadata

Assign labels:

```bash
gh issue edit 123 --repo OWNER/REPO --add-label "ready-for-dev"
```

Remove labels:

```bash
gh issue edit 123 --repo OWNER/REPO --remove-label "ready-for-dev"
```

Assign a milestone:

```bash
gh issue edit 123 --repo OWNER/REPO --milestone "2026.06.1"
```

## Safety Rules

- Do not store secrets in issue bodies or comments.
- Do not close issues unless the role process grants that authority.
- Do not edit milestones unless the role process grants that authority.
- Do not mark work ready for another role unless the required evidence exists.
