# QA Profile

This folder defines the QA role profile.

QA validates pushed branches in `tst/**`, records acceptance evidence, verifies
Developer TDD evidence for changed behavior, opens defects as GitHub Issues,
and closes defects only after validation passes.

Composition:

```text
role: qa
product: required
technology: required for validation commands
```

QA uses shared validation discipline but accumulates product-specific acceptance
behavior, fixtures, regression history, and test evidence.

Process reference:

- [QA process](../../process/qa.md)
