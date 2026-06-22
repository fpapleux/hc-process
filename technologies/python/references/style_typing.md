# Style, Typing, Naming, and Structure

Use this reference when deciding how Python code should look, be named, be
annotated, or be decomposed.

## Style

Follow PEP 8 and local style, with local consistency taking precedence inside
older files.

- Use 4 spaces for indentation.
- Prefer clear names over short names.
- Keep imports at the top, grouped as standard library, third-party, local.
- Avoid wildcard imports.
- Prefer one import per line.
- Prefer f-strings for formatting.
- Use UTF-8 and explicit encodings when reading or writing text.
- Comments explain why, not what.
- Delete stale comments.
- Prefer flat control flow and guard clauses when they improve readability.

## Naming

- Modules and CLI files: `snake_case.py`.
- Functions and variables: `snake_case`.
- Classes and dataclasses: `PascalCase`.
- Constants: `UPPER_CASE`.
- Booleans: `is_valid`, `has_errors`, `dry_run`, `should_update`.
- Tests: name expected behavior, e.g. `test_rejects_missing_required_field`.
- Avoid overloaded names such as `data`, `result`, or `item` when a precise
  domain name exists.
- Avoid abbreviations unless standard in the domain.

## Typing

Use type hints for all new public functions and for internal functions where
they clarify intent.

- Prefer built-in generics: `list[str]`, `dict[str, object]`, `set[int]`.
- Import abstract containers from `collections.abc`: `Mapping`, `Sequence`,
  `Iterable`, `Callable`.
- Use `Path` rather than raw strings for filesystem logic.
- Use dataclasses for structured records with behavior-light data.
- Prefer `@dataclass(frozen=True)` for immutable configuration or result types.
- Avoid mutable defaults; use `field(default_factory=list)` or equivalent.
- Avoid `Any` unless crossing an untyped boundary. Narrow it quickly.
- Use `TypedDict` or dataclasses for known JSON-like shapes when the structure
  matters.
- Use `NewType` or small value objects when mixing identifiers would be risky.
- Every `# type: ignore[...]` should be narrow and justified by context or tool
  limitation.

## Function design

- Keep functions small enough that their purpose is visible without scrolling.
- Make dependencies explicit through parameters instead of hidden globals.
- Return values instead of printing inside business logic.
- Raise exceptions from library functions; convert them to exit codes at the CLI
  boundary.
- Prefer pure functions for parsing, validation, transformation, and planning.
- Put I/O at the edges: CLI parsing, file reads/writes, network calls, database
  calls, and printing.
- Avoid boolean parameters that radically change behavior unless they are common
  operational flags such as `dry_run`.
- Avoid mutable global state. If a global cache or registry is justified, keep it
  internal, document why, and provide a way to reset it in tests.
