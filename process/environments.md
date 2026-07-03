# Product Environment Process

Products with mutable external systems use explicit environment lanes.

## Environment Lanes

```text
local/dev -> test/staging -> production
```

The target environment is controlled by role, product process, and scoped
credentials. It is not controlled by a casual `--prod` command-line flag.

## Platform And Application Segmentation

Separate platforms from application environments.

Platforms are shared host software capabilities such as databases, queues,
vector stores, identity providers, and observability stacks. Application
environments are product-specific source checkouts, branches, built artifacts,
deployed code, configuration, credentials, schemas, collections, namespaces, and
data roots.

Default rule:

- Use one production platform.
- Use one non-production platform.
- Segment development and test within the non-production platform when the
  platform supports safe logical separation.

Examples of non-production segmentation:

- separate database schemas;
- separate queues or topics;
- separate vector collections;
- separate buckets or prefixes;
- separate Git checkouts, branches, commits, and image/artifact tags;
- separate credentials and service identities;
- separate application config and data roots.

Create additional platform instances only when there is a recorded reason, such
as hard isolation, incompatible versions, capacity, security, or release
rehearsal fidelity.

## Role Ownership

Developer:

- Works against local/dev resources.
- Does not receive production credentials.
- Does not run production mutations.

QA:

- Works against test/staging resources.
- Does not receive production credentials.
- Validates behavior before release assembly.

Architect:

- Identifies environment, credential, deployment, and operational impact.
- Does not receive production credentials.
- Does not run environment mutations.

Release:

- Promotes validated release branches.
- Uses production credentials only through approved release tooling.
- Records production promotion evidence.

Operations Lead:

- Monitors production environment health through approved observability
  tooling.
- Does not receive ad-hoc production mutation access.
- Executes production changes only through documented runbook procedures
  (scaling, restart, failover).
- Uses production credentials only through approved operational tooling.
- Creates GitHub Issues for operational problems found in production.

Product Owner:

- Defines whether a product requires local, test, staging, and production
  environments.
- Ensures issues specify the environment expectation when it affects scope.

## External Service Example

For a product backed by a mutable external service:

```text
local service: developer implementation and local script checks
test service: QA validation and release rehearsal
prod service: release/operations only after approval gates
```

Service scripts do not default to production during development or QA.
Production execution is a release or operations action, not a developer
convenience flag.

## Script Requirements

Scripts that interact with mutable external systems:

- validate target environment before side effects;
- receive credentials from scoped configuration;
- expose dry-run or preview behavior when practical;
- record changed IDs, files, counts, and verification commands;
- fail closed when environment or credential state is unclear.
