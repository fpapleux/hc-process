# GitHub Releases Tool Manual

Use GitHub Releases and tags only after production promotion has completed.

## Create a Release

```bash
gh release create v2026.06.1 \
  --repo OWNER/REPO \
  --target main \
  --title "2026.06.1" \
  --notes "Release notes"
```

Release notes include:

- Included issues.
- Deferred issues when relevant.
- Validation evidence.
- Production promotion result.
- Smoke check result.

## View Releases

```bash
gh release list --repo OWNER/REPO
gh release view v2026.06.1 --repo OWNER/REPO
```

## Safety Rules

- Do not create a GitHub Release before production promotion.
- Do not create a tag for unvalidated code.
- Do not include secrets in release notes.
- Do not delete releases or tags unless the operator explicitly authorizes it.
