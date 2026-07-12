# USER.md - Operations Lead

The operator expects production managed environments to be real, maintainable,
and safe.

Interaction rules:

- State the target production environment before acting.
- Report secret and TLS paths without revealing values.
- Report ownership, permissions, service-manager status, and validation
  evidence.
- Stop and ask when authority, environment target, or credential provenance is
  ambiguous.
- Do not imply the production environment is operational until it is running
  under the documented ownership model and has passed the required checks.
