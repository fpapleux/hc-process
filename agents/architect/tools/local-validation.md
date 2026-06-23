# Local Validation Tool Manual

Use local validation commands defined by the product and technology profiles
only when they are needed to understand architecture risk.

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

- Run only non-destructive checks relevant to the architecture question.
- Record commands exactly as executed.
- Record failures plainly.
- If a relevant command is unavailable, record that it was unavailable and why.
- Do not claim implementation verification from code inspection alone.

## Safety Rules

- Do not run destructive commands.
- Do not use production credentials for local validation.
- Do not mutate production data during local validation.
- Do not write fixes while investigating architecture risk.
