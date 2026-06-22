# Packaging, Tooling, and Repository Discovery

Use this reference when setting up or discovering project commands, packaging,
formatting, linting, type checking, or dependency conventions.

## Project shape

For package-sized projects, prefer modern Python packaging:

```text
pyproject.toml
src/<package_name>/
    __init__.py
    ...
tests/
    test_*.py
```

For small script collections, keep reusable logic separate from entry points:

```text
scripts/
    task_name.py
src/ or lib/
    reusable_module.py
tests/
    test_task_name.py
```

Use `pyproject.toml` for build-system metadata and tool configuration when the
project has more than one script or will be tested/linted consistently.

## Command discovery protocol

Before inventing commands, inspect local conventions:

1. `pyproject.toml`
2. `Makefile`
3. `justfile`
4. `tox.ini`
5. `noxfile.py`
6. `.github/workflows/`
7. project README/contributing docs
8. existing scripts under `scripts/`

Prefer repository-defined commands over generic ones.

## Tooling baseline

Use the repository's existing tools first. If there is no project standard,
prefer:

- `ruff format` for formatting.
- `ruff check` for linting and import hygiene.
- `pytest` for tests.
- `mypy` for static typing where the project is typed or safety-critical.

For legacy code, introduce typing and linting gradually. Start with changed
files and widely imported utility modules. Do not reformat unrelated code in a
functional patch unless the user requested a formatting pass.

## Dependency policy

- Prefer the standard library for small scripts.
- Add third-party dependencies only when they materially reduce risk or
  complexity.
- Do not install packages without approval in managed or production workspaces.
- Keep dependency declarations in `pyproject.toml` or the repository-approved
  dependency file.
- Pin tooling versions when reproducibility matters.
- Avoid introducing a dependency for a one-line wrapper around the standard
  library.

## Validation report

When validating repository commands, report:

- command run;
- working directory;
- result;
- time if it was slow;
- whether failure appears pre-existing or caused by the change;
- narrower command used if full validation is too expensive.
