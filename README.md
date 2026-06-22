# Agent Operating Standards

This repository defines the operating standards for agent-managed work.

It is the source of truth for how agents are profiled, how work moves through
the product lifecycle, how technology-specific practices are applied, and how
product-specific context is attached to otherwise reusable roles.

The standards are built around one rule:

```text
Role controls authority.
Product controls intent.
Technology controls technique.
```

## Repository Structure

```text
.
  agents/       agent role profiles and profile composition rules
  process/      operating processes for product work
  security/     environment, secrets, and security follow-up
  tools/        canonical reusable tool manuals
  technologies/ technology-specific standards and practices
  products/     product-specific profiles and context
  README.md     repository overview
```

The `products/` folder may not exist yet. It is part of the intended structure
and will be added as product profiles are defined.

## Agents

[agents/](agents/README.md) defines reusable role profiles.

Current role profiles:

- Product Owner
- Developer
- QA
- Release

Each role profile defines authority, boundaries, inputs, outputs, handoffs, and
recurring operating checks. Role profiles do not carry product-specific intent
or technology-specific implementation detail.

See [agents/dimensions.md](agents/dimensions.md) for the role/product/technology
composition model.

## Process

[process/](process/overview.md) defines how work moves from idea to production.

Current process definitions:

- [Overview](process/overview.md)
- [Product Owner](process/product-owner.md)
- [Developer](process/developer.md)
- [QA](process/qa.md)
- [Release](process/release.md)
- [Product environments](process/environments.md)

The process documents define issue lifecycle, release planning, development,
QA, defect management, release assembly, production promotion, and environment
promotion.

## Security

[security/](security/README.md) defines environment and secret-handling
standards.

Current security notes:

- [Environment and secrets](security/environment.md)
- [Security todo](security/todo.md)

## Tools

[tools/](tools/README.md) contains canonical reusable tool manuals.

Tool documentation uses two layers:

- Canonical `tools/*.md` files explain how a tool works.
- Role `TOOLS.md` files grant permission and point only to the manuals that role
  can use.

At agent creation time, copy only the approved manuals into that agent's local
`tools/` folder. Agents must not receive instructions for tools or operations
outside their lane.

## Technologies

[technologies/](technologies/README.md) defines stack-specific practices.

Current profiles:

- [Python](technologies/python/README.md)

Planned examples:

- General coding standards.
- JavaScript.
- Shopify.
- Strapi CMS.
- Browser automation.
- Database migrations.

Technology profiles define technique: test commands, lint commands, packaging,
framework conventions, review checklists, and common failure modes. Technology
profiles do not grant authority.

## Products

`products/` will define product-specific context.

Examples:

- Product vision.
- Domain language.
- Roadmap and priorities.
- Repository and branch configuration.
- Deployment targets.
- Product-specific acceptance criteria.
- Regression history.

Product profiles define intent and accumulated product memory. Product profiles
do not grant authority by themselves.

Current product overlays:

- [Hearts & Colors Python](products/hearts-colors/overlays/python.md)

## Concrete Agent Profiles

A concrete agent profile is assembled from:

```text
role profile
+ product profile
+ one or more technology profiles
```

Example:

```text
agent: product-python-developer
role: agents/developer
product: products/product-name
technology:
  - technologies/general-coding
  - technologies/python
```

This keeps permissions, business intent, and implementation practices separate.
