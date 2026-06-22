# Testing and Test Planning

Use this reference when adding, changing, reviewing, or planning tests.

## Test strategy

Prefer a test pyramid:

- Many fast unit tests for pure logic.
- Fewer integration/service tests for boundaries.
- Very few expensive end-to-end tests for critical flows.

High-level tests should prove critical flows; they should not carry all
behavioral coverage.

## Test plan template

For non-trivial changes, define:

```text
Unit:
Integration/boundary:
Negative/edge:
Regression:
Smoke:
Commands to run:
```

## What to test

- Public behavior, not private implementation details.
- Happy path and meaningful edge cases.
- Invalid input and validation failures.
- Missing config, missing files, denied targets, and external failures.
- Regression case for each fixed bug when practical.

## Test design

- Test pure logic directly without shelling out.
- Test CLI behavior through `main(argv)` or subprocess when argument parsing or
  stdout/stderr is part of the contract.
- Use `tmp_path` for filesystem tests.
- Use monkeypatching for environment variables and external boundaries.
- Prefer fakes/stubs at boundaries when they make behavior clearer.
- Avoid excessive mocks that lock tests to implementation details.
- Keep tests deterministic; avoid production network/API calls.

## pytest conventions

- Use pytest unless the project already uses another framework.
- Use `test_*.py` files and `test_*` functions.
- Prefer `python -m pytest` or the repository's documented command.
- Use parametrization for repeated cases with different inputs.
- Keep fixture scope narrow unless expensive setup justifies broader scope.

## Validation levels

Run the smallest useful check first, then broaden:

1. Focused unit test for changed logic.
2. Focused integration or CLI smoke test.
3. Related test file/module.
4. Full test suite when risk, scope, or repository norms require it.
5. Lint/type/format checks before final handoff when available.

## Do not overstate validation

If a command was not run, say so. If a command failed because setup was missing,
report the exact failure and do not claim the code is verified.
