# Product Environment Process

Products with mutable runtime behavior or mutable external-system access use
explicit environment lanes.

Environment separation is mandatory for every product, platform, feature, tool,
and runtime we build now or in the future. Production and non-production are
separate runtime worlds. This is a process control, not a technology
preference.

## Environment Lanes

```text
local/dev -> test/staging -> production
```

The target environment is controlled by role, product process, separate runtime
instances, platform permissions, and scoped credentials. It is not controlled
by a casual `--prod` command-line flag.

A runtime instance has exactly one target environment. A service, UI, API,
worker, scheduler, CLI daemon, or other mutable runtime must not expose,
select, proxy, switch between, or manage both production and non-production.
Environment selection by route parameter, query string, dropdown, command-line
flag, or config row is not a valid control when the same running process can
reach both worlds.

There is no "trusted operator" exception. A runtime that can reach both worlds
is misdesigned even when the operator is expected to choose carefully, the UI
shows warnings, or the feature is intended only for previews.

For products that expose a runtime platform or service, the first released
runtime must stand up the non-production platform persistently before the
environment is called operational. After that point, the non-production
platform is maintained as an environment, not recreated only for ad-hoc tests.
Smoke checks for that maintained environment use approved non-production
credentials, TLS material, configuration, and service targets.

The non-production runtime must prove that it cannot access production. Smoke
evidence is incomplete unless it shows that non-production credentials,
configuration, runtime roots, databases, queues, object stores, logs, service
units, scheduler actions, and external service targets are non-production only.

Generated dummy credentials, generated dummy certificates, and ephemeral test
fixtures may be used only to validate test harnesses. They are not acceptable
evidence that a maintained non-production platform is operational.

Technical Lead owns creation and maintenance of non-production environment
material, including host-managed non-production material. Operations Lead owns
creation and maintenance of production environment material. Release consumes
approved material from the owning role to validate and promote the released
artifact. Release must not invent credentials, TLS keys, service accounts, or
trust material to unblock its own smoke checks.

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
- Deploy one production application/runtime instance per product service.
- Deploy one non-production application/runtime instance per product service.
- Segment development and test within the non-production platform and
  non-production runtime instance only when the platform supports safe logical
  separation.

The default rule is a floor, not a suggestion. A single UI/API/service process
that can reach both production and non-production is prohibited, even if it
stores an environment column, renders a warning label, requires a flag, or asks
the operator to choose carefully.

Examples of non-production segmentation:

- separate database schemas;
- separate queues or topics;
- separate vector collections;
- separate buckets or prefixes;
- separate Git checkouts, branches, commits, and image/artifact tags;
- separate credentials and service identities;
- separate application config and data roots.

Examples of required production/non-production separation:

- separate deployed service processes or containers;
- separate service-manager units and service users when locally hosted;
- separate runtime roots and source checkouts;
- separate config roots and `.env` files;
- separate credentials, service identities, and secret stores;
- separate databases, schemas, or database files with access scoped to one
  runtime world;
- separate queue/topic/bucket/prefix namespaces;
- separate log roots and backup roots;
- separate scheduler/cron ownership so non-production cannot trigger
  production scheduled work;
- separate UI/API endpoints or network bindings with no cross-environment
  proxying.

Filesystem, network, credential, and platform permissions must enforce the
wall. UI labels, documentation, operator memory, or "be careful" workflows are
not controls.

Create additional platform instances only when there is a recorded reason, such
as hard isolation, incompatible versions, capacity, security, or release
rehearsal fidelity.

Create fewer than the required production and non-production runtime instances
only when the product has no mutable runtime and no mutable external-system
access. If a product can mutate production data or production systems, missing
runtime separation is release-blocking.

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
- Must reject architecture that lets one runtime instance, UI, API, worker,
  scheduler, CLI daemon, database handle, or service identity reach both
  production and non-production.
- Records missing environment separation as release-blocking architecture
  rework, not as a cosmetic documentation issue.

Release:

- Promotes validated release branches.
- Uses production credentials only through approved release tooling.
- Records production promotion evidence.
- Consumes Technical Lead-approved non-production material and
  Operations-approved production material during smoke checks and deployment
  validation.
- Does not create or approve host-managed credentials, TLS material, service
  identities, or secret roots.
- Must refuse release promotion when non-production can access production
  runtimes, credentials, data stores, logs, service units, scheduler actions, or
  external production targets.
- Must refuse release promotion when any unresolved readiness finding is
  classified `release-blocking`.
- May proceed with an `operator-waivable` readiness finding only when the
  operator explicitly approves the risk in a waiver record and the follow-up
  issue is recorded.

Technical Lead:

- Provisions and maintains non-production environment material outside Git:
  secret roots, credentials, TLS material, service-manager configuration,
  runtime directories, dashboards, alerts, and runbooks.
- Owns development, test, staging, and maintained non-production runtime
  readiness.
- Provisions non-production runtime material separately from production. A
  shared service account, shared runtime root, shared config file, shared
  DB/log root, or shared scheduler that can cross the production/non-production
  boundary is not acceptable non-production material.
- Creates GitHub Issues for non-production environment problems that block
  development, QA, release rehearsal, or non-production smoke validation.

Operations Lead:

- Provisions and maintains production environment material outside Git: secret
  roots, credentials, TLS material, service-manager configuration, runtime
  directories, dashboards, alerts, and runbooks.
- Monitors production environment health through approved observability
  tooling.
- Does not receive ad-hoc production mutation access.
- Executes production changes only through documented runbook procedures
  (scaling, restart, failover).
- Uses production credentials only through approved operational tooling.
- Creates GitHub Issues for operational problems found in production.
- Must not create or mutate non-production environment material unless the
  operator explicitly delegates a specific support task through Technical Lead.

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
- refuse to run when a non-production invocation can resolve production
  credentials, production runtime roots, production database/log paths, or
  production external-service targets.
