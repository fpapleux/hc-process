# Local Validation Tool Manual

Use local validation commands defined by the product and technology profiles.

This manual defines the evidence format. The technology profile defines the
actual commands.

## Evidence Format

Record exact commands and results:

```text
Verification:
- <command>: passed
- <command>: failed, reason: <summary>
- <manual check>: not run, reason: <reason>
```

## Required Behavior

- Run the relevant test, lint, typecheck, build, or smoke commands for the
  technology profile.
- Record commands exactly as executed.
- Record failures plainly.
- If a relevant command is unavailable, record that it was unavailable and why.
- Do not claim verification from code inspection alone.

## Safety Rules

- Do not run destructive commands unless the role process permits them.
- Do not use production credentials for local validation.
- Do not mutate production data during local validation.
