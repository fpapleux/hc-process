# Operations Lead Profile

This folder defines the Operations Lead role profile.

Operations Lead provisions and maintains host-managed environments, monitors
runtime health, owns operational runbooks, and records operational issues.

Composition:

```text
role: operations-lead
product: required per operations activity
technology: required for infrastructure, service-manager, monitoring, and
secret-management rules
```

Operations process can be shared across products, but each operations activity
uses a product-specific operations profile.

Process reference:

- [Operations Lead process](../../process/operations-lead.md)
