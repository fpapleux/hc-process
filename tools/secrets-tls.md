# Secrets And TLS Tool Manual

Manage credentials and TLS material as host-managed environment material
outside Git.

## Required Behavior

- Create or select an approved secret root outside Git.
- Store one secret value per file when the product uses file-backed secret
  references.
- Store TLS certificates and private keys in the paths required by the product
  registry or runbook.
- Set ownership and permissions so only approved service identities and
  operators can read secrets and private keys.
- Record names, paths, owners, and rotation expectations without values.

## Verification

Use metadata checks, not value dumps:

```bash
find SECRET_ROOT -maxdepth 3 -type f -printf '%p %s bytes mode=%m owner=%u:%g\n'
```

TLS certificate metadata may be inspected without printing private keys:

```bash
openssl x509 -in PATH/TO/cert.crt -noout -subject -issuer -dates
```

## Safety Rules

- Do not commit secrets, tokens, passwords, or TLS private keys.
- Do not paste secret values into issues, release notes, logs, or chat.
- Do not use generated dummy material as operational readiness evidence.
- Do not reuse production credentials in non-production environments.
