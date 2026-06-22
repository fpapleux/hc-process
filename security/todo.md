# Security Todo

## Secret Inventory

Create a map of secret-bearing files across the system.

Scope for the inventory:

- `.env`
- `.env.*`
- `environment.local`
- config files containing tokens, API keys, passwords, credentials, private
  endpoints, or webhook secrets
- product-specific secret files
- OpenClaw config or agent files containing secret material

Inventory rules:

- Record file paths and owning product/system.
- Do not copy secret values into the inventory.
- Classify each file by required role access.
- Identify files that must move to the operator vault.
- Identify values that must be split into role/product scoped config.
- Identify obsolete or duplicated secrets.

This inventory will be performed product by product during product review.

## Environment Restructure

Planned work:

- Move the operator vault out of the OpenClaw directory.
- Define role-scoped config generation.
- Define product-scoped environment files.
- Define test/staging environment requirements for products with mutable
  external systems such as CMS.
- Remove production selection from developer/QA scripts.
