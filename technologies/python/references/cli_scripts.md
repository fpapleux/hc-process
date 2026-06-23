# CLI and Script Architecture

Use this reference when creating or reviewing Python scripts, CLIs, automation
jobs, or agent-facing command-line tools.

## Generic CLI Pattern

The generic Python standard is:

1. State what the script does, what it prints, and whether it can be imported.
2. Keep domain logic importable.
3. Keep CLI parsing and process exit behavior at the boundary.
4. Validate arguments and configuration before side effects.
5. Inject dependencies explicitly instead of reading global state inside domain
   logic.
6. Print documented data output to stdout.
7. Print diagnostics to stderr.
8. Return non-zero exit codes for failures.

Prefer the project's existing CLI architecture. For standalone automation
scripts, use importable domain logic and a thin CLI adapter.

Do not make product-specific, runtime-specific, or organization-specific
assumptions in generic Python code. Those belong in product or runtime overlays.

## Atomic Purpose Taxonomy

Every script has one main purpose. Classify the script before writing or
changing it.

### Executor

An executor performs one operation on one target.

Use an executor for:

- create one record;
- update one record;
- transform one file;
- sync one object;
- validate one target before mutation;
- run one bounded action.

Executor rules:

- Accept one target or one target descriptor.
- Validate the target before side effects.
- Perform one domain action.
- Return or print a structured result.
- Support `--dry-run` when the operation has side effects.
- Do not loop over many independent targets.
- Do not schedule itself.
- Do not produce broad reports beyond its own result.

Executor naming examples:

```text
create_product.py
update_document.py
process_image.py
sync_record.py
```

### Batch Runner

A batch runner loops over executors.

Use a batch runner for:

- applying one executor to many targets;
- reading target lists from JSON, CSV, or a queue;
- collecting per-target success and failure;
- resuming or retrying independent items.

Batch runner rules:

- Reuse executor logic instead of duplicating domain logic.
- Treat each target as independently as possible.
- Collect per-item results.
- Continue after item-level failures only when targets are independent.
- Produce a batch summary.
- Do not own scheduling.
- Do not hide executor failures.

Batch runner naming examples:

```text
batch_process_images.py
batch_sync_records.py
run_document_updates.py
```

### Validator

A validator is read-only verification.

Use a validator for:

- checking output exists;
- checking records match expected state;
- checking schema, config, or file shape;
- verifying release or migration results;
- proving acceptance criteria.

Validator rules:

- Do not mutate persistent state.
- Return non-zero when validation fails.
- Print clear diagnostics to stderr.
- Print machine-readable result to stdout when agents consume it.
- Record what was checked.

Validator naming examples:

```text
validate_export.py
check_document_state.py
verify_release.py
```

### Reporter

A reporter summarizes state, results, or evidence.

Use a reporter for:

- summarizing batch results;
- generating audit evidence;
- producing human-readable status;
- aggregating validation outputs.

Reporter rules:

- Prefer read-only behavior.
- Keep reporting separate from mutation.
- State data sources and time range.
- Do not silently repair data.
- Do not schedule itself.

Reporter naming examples:

```text
report_batch_results.py
summarize_validation.py
generate_release_report.py
```

### Job / Orchestrator

A job owns scheduling or orchestration.

Use a job for:

- scheduled runs;
- multi-step workflows;
- coordinating executors, validators, and reporters;
- managing checkpoints across steps.

Job rules:

- Call executors, validators, and reporters rather than embedding all logic.
- Own ordering, retries, checkpointing, and notifications.
- Make each step visible in logs or output.
- Fail closed when required steps fail.
- Do not hide which operation failed.
- Do not bypass role, product, or environment permissions.

Job naming examples:

```text
job_nightly_sync.py
orchestrate_release_validation.py
schedule_report_generation.py
```

## Composition Rule

Do not mix script types casually.

Bad pattern:

```text
one script reads a CSV, mutates many records, validates them, generates a report,
and schedules itself
```

Better pattern:

```text
executor: update_one_record.py
batch runner: batch_update_records.py
validator: validate_record_state.py
reporter: report_update_results.py
job: job_update_records.py
```

The job coordinates. The batch runner loops. The executor mutates one target.
The validator verifies. The reporter summarizes.

## Generic Skeleton

```python
"""Processes one target and prints JSON to stdout.

The `process_target` function can be imported by tests or another script.
"""

from __future__ import annotations

import argparse
import json
import sys
from pathlib import Path


def process_target(target: Path, dry_run: bool = False) -> dict[str, object]:
    if not target.exists():
        raise ValueError(f"target does not exist: {target}")

    # Domain work here. Keep argparse Namespace objects out of this function.
    return {"target": str(target), "dry_run": dry_run, "status": "ok"}


def main(argv: list[str] | None = None) -> int:
    parser = argparse.ArgumentParser(
        description="Processes one target and prints JSON to stdout."
    )
    parser.add_argument("target", type=Path, help="target path")
    parser.add_argument(
        "--dry-run",
        default=False,
        action="store_true",
        help="preview without making changes",
    )

    args = parser.parse_args(argv)

    try:
        result = process_target(args.target, dry_run=args.dry_run)
    except ValueError as exc:
        print(f"Error: {exc}", file=sys.stderr)
        return 2

    print(json.dumps(result, indent=2), file=sys.stdout)
    return 0


if __name__ == "__main__":
    raise SystemExit(main())
```

This `main(argv)` style is the generic default because it is simple to test.
If a project already uses another convention, preserve local consistency.

## Importable Domain Function

- Name the function after the action: `process_file`, `sync_records`,
  `export_rows`, `validate_config`, `build_payload`.
- Pass prepared dependencies into the function: client objects, paths, config
  values, IDs, and options.
- Keep argparse `Namespace` objects out of domain functions.
- Return the domain result: dict, list, ID, object, or summary.
- Raise exceptions from domain logic; convert expected user/config errors to
  exit codes at the CLI boundary.

## CLI Boundary

- Use `argparse` unless the project already uses another CLI framework.
- Use `argparse.ArgumentTypeError` for malformed CLI values when practical.
- Support `--dry-run` for batch, destructive, file-changing, or expensive
  operations.
- Avoid environment-selection flags such as `--prod` in generic scripts.
  Environment routing belongs to product process, role authority, and approved
  deployment tooling.
- If an operation can target persistent state, the target environment must come
  from role-scoped configuration or product process, not a casual CLI switch.

## Configuration Loading

Generic Python code must not assume access to the operator vault or global
runtime environment.

Configuration rules:

- Load configuration through the mechanism documented by the product or runtime
  profile.
- Validate required values before side effects.
- Inject configuration into domain functions.
- Treat missing configuration as a startup failure.
- Do not hard-code credentials, tokens, production paths, service URLs, or
  account identifiers.

Runtime-specific environment behavior belongs in an approved runtime or product
overlay.

## Output Contracts

Match output to the script's purpose:

- Query/list/get scripts: pretty JSON to stdout.
- Create scripts: print the new ID or documented identifier to stdout.
- Update/set scripts: JSON result to stdout when useful.
- Batch scripts: progress and summary may be human-readable; write a report file
  when the operation spans many records.
- Validation and usage failures: stderr plus exit code `2`.
- Runtime failures after valid input: stderr plus exit code `1`.

Do not mix diagnostic text into stdout when stdout is documented as JSON.

## Side-Effect Safeguards

- Query or inspect current state before changing state.
- Validate target names, IDs, field names, paths, and modes before saving.
- For file edits, create a backup or use staging when the file is a record.
- For batch changes, collect per-item errors and continue only when items are
  independent.
- Use local, test, staging, dry-run, backup, or preview workflows when project
  rules require them.

## Handoff Checklist

Before handing off a script:

- `python3 script.py --help` works when the script has a CLI.
- Invalid target syntax fails before side effects.
- Required configuration failure is clear.
- `--dry-run` works when applicable.
- Importing the file exposes domain logic without executing the CLI.
- Stdout matches the documented contract.
- Diagnostics go to stderr.
