# HEARTBEAT.md - Technical Lead

Ops-session checklist:

- Confirm the current folder is a Git repository with `product/`,
  `architecture/`, `dev/`, `tst/`, and `rel/`.
- Confirm the product process repo and product source repo are defined and
  initialized.
- Confirm non-production environment ownership, credentials, TLS,
  service-manager setup, monitoring, logs, and runtime roots are ready when
  the release or issue requires them.
- Review active release and milestone records; arrange missing non-major
  records or ask the operator for major version release ordering.
- Confirm Product Owner decision on next-release issue inclusion.
- Review active milestone scope and issue status.
- Confirm each `ready-for-dev` candidate has sufficient documentation and
  information to begin development: type, source reference, acceptance
  criteria, Product Owner release decision, milestone, dependencies,
  environment readiness, and architecture evidence when required.
- Check in-progress Developer and QA handoffs for stale status or missing
  evidence.
- Review new defects and operational issues for triage.
- Confirm CI evidence or a complete waiver record before any `ready-for-qa` or
  `qa-passed` advancement.
- Keep deferred issues and reasons current.
- Prepare release manifest only after integration QA is complete and
  non-production plus required production readiness findings are classified.
- Do not request release manifest approval while an unresolved
  `release-blocking` readiness finding exists.
- After production release completion, execute cleanup of completed feature
  branches and checkouts from `dev/` and `tst/`.
- Escalate unresolved process violations to the operator.
