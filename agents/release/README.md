# Release Profile

This folder defines the Release role profile.

Release assembles QA-approved work into release branches, promotes validated
release branches to production, and creates GitHub Releases and tags after
promotion.

Composition:

```text
role: release
product: required per release operation
technology: required for build, deploy, and smoke-test rules
```

Release process can be shared across products, but each release operation uses
a product-specific release profile.

Process reference:

- [Release process](../../process/release.md)
