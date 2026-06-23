# Python Coding Operating Model

This technology profile defines Python technique for agents whose role and
product profiles authorize Python work.

## Prime directive

Write Python that is boring, explicit, testable, reversible, and safe to run.
Optimize for correctness and maintainability before speed of implementation.

Prefer small, composable functions over clever abstractions. If the
implementation is hard to explain, redesign it.

## Atomic purpose

Every script has one main purpose.

Classify scripts before writing or changing them:

- Executor: one operation on one target.
- Batch runner: loops over executors.
- Validator: read-only verification.
- Reporter: summarizes state, results, or evidence.
- Job: owns scheduling or orchestration.

Do not combine these responsibilities in one script unless the local product
standard explicitly requires it. Read `references/cli_scripts.md` for the full
taxonomy.

## When not to use this profile

Do not load extra references for:

- short conceptual Python explanations;
- one-line syntax questions;
- reading code without editing or reviewing it;
- non-Python work unless Python tooling or generated Python code is involved.

## Core loop

1. Inspect local conventions before inventing new ones.
2. Identify the smallest safe change.
3. Separate pure logic from I/O and side effects.
4. Write or update tests first when behavior changes, unless a clear reason
   makes a different order safer.
5. Implement the change.
6. Run the smallest relevant validation.
7. Run broader project validation when the risk justifies it.
8. Report commands run, results, and remaining risk.

## Decision tree

- Creating or modifying a CLI/script: read `references/cli_scripts.md`.
- Adding or fixing tests, or planning validation: read `references/testing.md`.
- Handling files, subprocesses, databases, APIs, secrets, user input, or other
  risky boundaries: read `references/validation_security.md`.
- Making style, typing, naming, or structure decisions: read
  `references/style_typing.md`.
- Setting up packaging, linting, formatting, type checking, or dependency
  conventions: read `references/packaging_tooling.md`.
- Working in product-specific or runtime-specific automation: load the relevant
  product or runtime overlay, not generic Python alone.
- If multiple branches apply, read only the narrowest references needed.

## Non-negotiable rules

- Do not hide failures. Return non-zero from failed CLIs, print actionable
  diagnostics to stderr, and preserve enough context to debug.
- Do not use random fixes during troubleshooting. Identify the cause, gather
  evidence, choose one fix, execute it, then verify end-to-end.
- Do not hand over untested code. Run the relevant script, unit tests, lint/type
  checks, or a focused smoke test before saying it is ready.
- Do not depend on ambient secrets. Load configuration through documented
  configuration paths and validate required values before use.
- Do not make irreversible changes first. Use dry-run, local, staging, backups,
  or preview modes whenever the operation can affect persistent state.
- Do not use `assert` for runtime validation or security checks. Assertions may
  be optimized away and are for internal invariants and tests, not user input.

## Definition of done

Before declaring code ready, verify:

- The script or module has an atomic purpose.
- The code has a single clear responsibility.
- Side effects are explicit and guarded when appropriate.
- Inputs are validated before effects.
- Secrets are loaded, not hard-coded.
- Errors fail loudly and use non-zero exit codes for CLIs.
- Output contracts are documented and stable.
- New or changed behavior is tested.
- Importing the module does not execute side effects.
- CLI help works when a CLI exists.
- The final report states what was run and what passed.

## Minimal script pattern

For scripts, prefer importable domain logic and a thin CLI adapter unless the
local project already uses a different convention. Read
`references/cli_scripts.md` before creating or reviewing CLI scripts.

```python
## SYNOPSIS
##
## Prints JSON to stdout. Can be run as a CLI or imported.

import os, sys, json
import argparse


def do_task(dependency, target: str) -> dict:
    # Validate first, then perform domain work.
    return {"target": target, "status": "ok"}


if __name__ == "__main__":

    parser = argparse.ArgumentParser(description="Does one task.")
    parser.add_argument("target", type=str, help="target identifier")
    parser.add_argument("--local", default=False, action="store_true")

    args = parser.parse_args()
    result = do_task(None, args.target)
    print(json.dumps(result, indent=2), file=sys.stdout)
```

## Test plan template

For non-trivial changes, state or execute:

```text
Unit:
Integration/boundary:
Negative/edge:
Regression:
Smoke:
Commands to run:
```

## Completion report template

```text
Changed:
Validation run:
Result:
Not run:
Risk / follow-up:
```

## Common AI failure modes to avoid

- Do not claim tests passed unless you ran them.
- Do not add broad abstractions for one-off code.
- Do not reformat unrelated files.
- Do not silently skip validation because setup is inconvenient.
- Do not write implementation before understanding existing conventions.
- Do not use mocks when a simple fake, fixture, or pure unit test is clearer.
- Do not trust model memory over repository-local instructions and commands.

## Boundaries

This profile defines how to write Python. It does not grant permission to:

- access the operator vault;
- read global environment files;
- use shared skills;
- call external services, databases, email, or production APIs;
- mutate product data;
- deploy code.

Those permissions must come from role, product, environment, and tool profiles.

## Source-backed principles

- PEP 20 / PEP 8: readability, explicitness, simplicity, and consistency.
- Anthropic Agent Skills: keep top-level skill content lean; use references for
  progressive disclosure; test and iterate from real failures.
- OpenAI skill eval guidance: define success criteria and test skill behavior
  with concrete prompts, deterministic checks, and rubric checks.
- GitHub Copilot / AGENTS.md guidance: document exact validation commands,
  project layout, and definition of done for coding agents.
- Vercel agent evals: broad always-needed guidance may need a short passive
  project-level pointer because optional skill retrieval can under-trigger.
- OWASP / NIST SSDF: validate untrusted input, fail securely, avoid leaking
  sensitive details, use least privilege, and integrate security into the SDLC.
