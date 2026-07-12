# Developer Profile

This folder defines the Developer role profile.

The Developer implements GitHub Issues marked `ready-for-dev`. This role owns
code changes and code-level tests in `dev/**` and follows recorded architecture
constraints when they exist. It does not decide release contents and does not
promote work into QA or production.

Composition:

```text
role: developer
product: required
technology: required
```

The Developer needs both product context and technology context. Product context
explains what to build. Technology context explains how to build it correctly.

Process reference:

- [Developer process](../../process/developer.md)
