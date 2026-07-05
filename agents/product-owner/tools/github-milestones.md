# GitHub Milestones Tool Manual

Use GitHub Milestones as the planning container for releases.

Milestones track planned releases. GitHub Releases and tags record completed
production releases.

Product Owner uses milestones to record intended release names and priority
input. Release opens the planning milestone at Technical Lead request. The
Technical Lead owns issue assignment after Product Owner and operator priority
input.

## Create a Milestone

```bash
gh api \
  --method POST \
  repos/OWNER/REPO/milestones \
  -f title="2026.06.1" \
  -f description="Planned release scope" \
  -f due_on="2026-06-30T00:00:00Z"
```

## List Milestones

```bash
gh api repos/OWNER/REPO/milestones
```

## Assign an Issue to a Milestone

```bash
gh issue edit 123 --repo OWNER/REPO --milestone "2026.06.1"
```

## Safety Rules

- Do not create multiple active release milestones unless the process records an
  explicit exception.
- Do not create or edit planning milestones unless the Technical Lead or
  operator explicitly delegates that action for this product.
- Do not assign, defer, or remove issues from a milestone without Technical
  Lead coordination and recorded reason.
- Do not use GitHub Releases for planning.
