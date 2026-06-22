# Validation, Security, and Data Safety

Use this reference when code touches files, subprocesses, databases, APIs,
credentials, user input, or persistent state.

## Validate before side effects

- Validate required arguments and configuration.
- Validate file existence, extension, encoding, size, and expected schema.
- Validate identifiers, enum values, statuses, locales, and target environments.
- Validate permissions or access assumptions before long-running work.
- Validate that update payloads contain at least one meaningful change.

Prefer explicit validation errors over downstream tracebacks when bad user input
is expected.

## Untrusted input

- Treat external data as untrusted until validated, including files, databases,
  APIs, queues, environment variables, and command-line args.
- Prefer allowlist validation for expected types, ranges, enum values, formats,
  and paths.
- Do not rely on denylist validation for hazardous input.
- Normalize/canonicalize text and paths before validation when equivalent forms
  could bypass checks.
- Do not use `assert` for runtime validation or security checks.

## Secrets and config

- Do not hard-code credentials, tokens, hostnames, or production paths.
- Load configuration through the mechanism documented by the project.
- Validate mandatory configuration at startup and fail before side effects.
- Redact secrets and sensitive data from logs, test snapshots, and errors.

## Data access

- Use parameterized queries or the framework's safe query builder.
- Never assemble SQL or structured queries by concatenating untrusted input.
- Use least-privilege service accounts and tokens.
- Close connections promptly.

## Command execution

- Prefer library APIs over shell commands.
- If subprocesses are necessary, pass args as a list.
- Avoid `shell=True`.
- Validate every user-derived argument.
- Set timeouts and capture diagnostics when useful.

## Filesystem safety

- Never overwrite a file of record without backup or explicit confirmation.
- Write generated output to temp/staging first.
- Use atomic writes for important files: write temp, flush/fsync when needed,
  then replace.
- Preserve source metadata for backups with `shutil.copy2`.
- Prefer quarantine/trash over permanent delete when the domain allows it.
- Resolve user-provided paths and reject unexpected target directories for
  destructive operations.

## Error behavior

- Fail securely when authorization, configuration, validation, or security state
  is uncertain.
- Do not expose internal stack traces, credentials, session identifiers, account
  data, or infrastructure details in user-facing errors.
- Keep try/except blocks narrow.
- Catch broad `Exception` only at boundaries that must collect independent job
  failures; record exception type and message.
