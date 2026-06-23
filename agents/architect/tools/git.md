# Git Tool Manual

Use Git to inspect local checkouts, branches, and history during architecture
review.

## Required Checks

Show current branch and local state:

```bash
git status --short --branch
```

Fetch remote refs without changing local files:

```bash
git fetch origin
```

Inspect recent history:

```bash
git log --oneline --decorate -n 10
```

Inspect changes:

```bash
git diff
```

## Safety Rules

- Do not commit.
- Do not push.
- Do not merge branches.
- Do not reset local state.
- Do not delete branches.
- Do not force-push.
- Do not commit secrets.
