# 0008 Build Web UI In Parallel With CLI

## Decision

Wildwest should start building a web server and web UI in parallel with the CLI.

For frontend design guidance, install and use the global Codex skill `design-taste-frontend` from `Leonxlnx/taste-skill`, installed locally under `~/.codex/skills/taste-skill`.

## Reason

The CLI proves the Linux/operator primitive, but Wildwest will need a control surface for inspecting agents, submitting work, reviewing logs, and eventually approving privileged actions.

Starting the web track early helps keep the product shape visible while the CLI hardens.

## Consequences

- Future web UI work should invoke the installed frontend design skill when appropriate.
- The web UI should not replace the CLI; both should expose the same core concepts.
- The first web server should stay close to the local control-plane model rather than becoming a broad SaaS platform too early.

## Open Questions

- Which framework should the web server use?
- Should the first UI be local-only, authenticated over localhost, or network-accessible?
- Should the web server talk to agents through the filesystem spool first or through a Wildwest daemon/API?

