# GitHub Pull Requests Tool Manual

Use GitHub Pull Requests to inspect proposed implementation and comment on
architecture fit.

## View Pull Requests

```bash
gh pr view 123 --repo OWNER/REPO
```

List pull requests:

```bash
gh pr list --repo OWNER/REPO
```

Comment on a PR:

```bash
gh pr comment 123 --repo OWNER/REPO --body "Comment body"
```

## Architecture Review Comment

Record:

- Whether the implementation follows the architecture note.
- Any boundary, contract, dependency, migration, or operational concern.
- Required changes before QA or release.
- Any accepted deviation from the original architecture note.

## Safety Rules

- Do not create pull requests.
- Do not merge pull requests.
- Do not close pull requests.
- Do not change the base branch.
- Do not use PR comments to bypass GitHub Issue status, QA evidence, or release
  approval.
