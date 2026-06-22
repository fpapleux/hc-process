# Product Environment Process

Products with mutable external systems use explicit environment lanes.

## Environment Lanes

```text
local/dev -> test/staging -> production
```

The target environment is controlled by role, product process, and scoped
credentials. It is not controlled by a casual `--prod` command-line flag.

## Role Ownership

Developer:

- Works against local/dev resources.
- Does not receive production credentials.
- Does not run production mutations.

QA:

- Works against test/staging resources.
- Does not receive production credentials.
- Validates behavior before release assembly.

Release:

- Promotes validated release branches.
- Uses production credentials only through approved release tooling.
- Records production promotion evidence.

Product Owner:

- Defines whether a product requires local, test, staging, and production
  environments.
- Ensures issues specify the environment expectation when it affects scope.

## CMS Example

For a CMS-backed product:

```text
local CMS: developer implementation and local script checks
test CMS: QA validation and release rehearsal
prod CMS: release/operations only after approval gates
```

CMS scripts do not default to production during development or QA. Production
execution is a release or operations action, not a developer convenience flag.

## Script Requirements

Scripts that interact with mutable external systems:

- validate target environment before side effects;
- receive credentials from scoped configuration;
- expose dry-run or preview behavior when practical;
- record changed IDs, files, counts, and verification commands;
- fail closed when environment or credential state is unclear.
