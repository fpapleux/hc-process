# Git Tool Manual

Use Git to manage local checkouts, branches, commits, and pushes.

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

## Create a Feature Checkout

Create a checkout under the phase folder assigned by the role:

```bash
git clone git@github.com:OWNER/REPO.git dev/123-short-slug
cd dev/123-short-slug
git switch -c 123-short-slug origin/main
```

## Commit and Push

Review changes:

```bash
git status --short
git diff
```

Commit with the GitHub Issue number:

```bash
git add .
git commit -m "Implement short scope for #123"
git push -u origin 123-short-slug
```

## Update a Test Checkout

Update a checkout to exactly match the pushed branch:

```bash
git fetch origin
git reset --hard origin/123-short-slug
```

This discards local changes. Use only in phase checkouts where the role process
permits replacing local state from the remote branch.

## Safety Rules

- Do not use `git reset --hard` unless the role process explicitly permits it
  for the current folder.
- Do not delete branches unless the role process explicitly permits it.
- Do not merge branches unless the role process explicitly permits it.
- Do not force-push unless the operator explicitly authorizes it.
- Do not commit secrets.
