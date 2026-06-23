# Agent Profile Dimensions

Agent effectiveness depends on separating authority, product context, and
technical practice. These dimensions are orthogonal and are composed into a
complete agent profile.

```text
agent profile =
  role profile
  + product profile
  + technology profile
```

The rule:

```text
Role controls authority.
Product controls intent.
Technology controls technique.
```

## Role Dimension

The role dimension defines what an agent is allowed to do.

Examples:

- Product Owner.
- Architect.
- Developer.
- QA.
- Release.

The role profile defines:

- Folder access.
- GitHub permissions.
- Required inputs.
- Required outputs.
- Handoff rules.
- Actions the agent must not perform.

Role profiles grant or deny authority. Technology profiles do not grant
authority.

## Product Dimension

The product dimension defines what the agent is working on and what product
memory it carries.

Product context includes:

- Product vision.
- User workflows.
- Roadmap and priorities.
- Domain language.
- Scope boundaries.
- Release history.
- Product-specific acceptance criteria.
- Product-specific regression history.
- Deployment targets and operational constraints.

Product context accumulates over time. Product Owner profiles are strongly
product-specific because they carry vision, priority, and scope. Architect,
Developer, and QA agents use product context while assigned to a product.
Release agents use product context through release profiles.

## Technology Dimension

The technology dimension defines how work is performed in a stack.

Examples:

- General coding standards.
- Python.
- JavaScript.
- E-commerce platform integrations.
- Content management systems.
- Database migrations.
- Browser automation.

Technology profiles define:

- Coding standards.
- Test commands.
- Lint commands.
- Package manager rules.
- Dependency management.
- Framework conventions.
- File layout conventions.
- Review checklist.
- Common failure modes.
- Build and packaging rules.

Technology profiles do not grant write access. A Python developer and a
JavaScript developer may follow different technical practices, but both remain
bound by the Developer role's authority.

## Composition Examples

Product-specific Python developer:

```text
agent: product-python-developer
role: developer
product: product-name
technology:
  - general-coding
  - python
```

Product-specific architect:

```text
agent: product-architect
role: architect
product: product-name
technology:
  - general-coding
  - python
  - infrastructure
```

Product-specific frontend QA:

```text
agent: product-frontend-qa
role: qa
product: product-name
technology:
  - general-qa
  - javascript
  - browser-automation
```

Cross-product release agent with scoped profiles:

```text
agent: release-manager
role: release
product profile selected per release:
  - product-name
technology profile selected per release:
  - ecommerce-platform
  - javascript
```

The release agent can serve multiple products only by loading one product
release profile at a time.

## Impact by Role

Product Owner:

- Product axis is strong.
- Technology axis is light.
- Uses technology labels to route work, such as `tech:python`,
  `tech:javascript`, `tech:ecommerce`, or `tech:cms`.
- Does not apply implementation standards directly.

Architect:

- Product axis is strong.
- Technology axis is strong.
- Uses product context to preserve system boundaries and intent.
- Uses technology context to choose direction, constraints, and review focus.
- Does not implement code or decide release scope.

Developer:

- Product axis is strong.
- Technology axis is strong.
- Uses product context to understand intent.
- Uses technology context to implement correctly.
- Executes inside one product and one feature branch at a time.

QA:

- Product axis is medium to strong.
- Technology axis is medium.
- Uses shared QA discipline.
- Uses product-specific acceptance criteria and regression history.
- Uses technology-specific validation tools and commands.

Release:

- Product axis is medium.
- Technology axis is medium to strong.
- Uses standard release process across products.
- Uses product-specific repository, branch, deployment, and credential profile.
- Uses technology-specific build, packaging, deploy, and smoke-test rules.

## Profile Folder Direction

This folder can evolve into:

```text
agents/
  README.md
  dimensions.md
  roles/
    product-owner.md
    developer.md
    qa.md
    release.md
  technologies/
    general-coding.md
    python.md
    javascript.md
    ecommerce-platform.md
    content-management-system.md
  products/
    product-name.md
```

Concrete agents are assembled by referencing one role profile, one product
profile, and one or more technology profiles.
