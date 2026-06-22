# GitHub Pull Requests Tool Manual

Use GitHub Pull Requests to expose branch changes for review and release
assembly when the product process requires PRs.

## View Pull Requests

```bash
gh pr view 123 --repo OWNER/REPO
```

List pull requests:

```bash
gh pr list --repo OWNER/REPO
```

## Create a Draft Pull Request

```bash
gh pr create \
  --repo OWNER/REPO \
  --draft \
  --base main \
  --head 123-short-slug \
  --title "Implement short scope for #123" \
  --body "PR body"
```

The PR body records:

- GitHub Issue number.
- Scope implemented.
- Verification commands and results.
- Known limitations.

## Update a Pull Request

```bash
gh pr edit 123 --repo OWNER/REPO --body "Updated body"
```

Comment on a PR:

```bash
gh pr comment 123 --repo OWNER/REPO --body "Comment body"
```

## Safety Rules

- Do not merge PRs unless the role process grants that authority.
- Do not close PRs unless the role process grants that authority.
- Do not change the base branch unless the role process grants that authority.
- Do not use PRs to bypass GitHub Issue status or QA evidence.
