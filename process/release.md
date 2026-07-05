# Release Process

Release agents assemble tested work into a release branch, promote validated
release branches to production, and create GitHub Releases and tags after
production promotion.

Release planning uses GitHub Milestones and release tracking issues. Those are
planning records, not GitHub Releases. A GitHub Release or tag must not be
created until production promotion is complete.

## Opening a Planning Release

At Technical Lead request, Release opens the planning release record in GitHub:

- Create or select one active GitHub Milestone for the planned release.
- Create a release tracking issue when the product uses one.
- Record the milestone name, intended release name, and initial empty scope.
- Do not assign feature or defect issues to the milestone unless the Technical
  Lead explicitly requests that assignment.
- Do not create a GitHub Release or tag.

The Technical Lead owns release scope selection after Product Owner and
operator priority input. Release owns the planning record, release branch,
promotion, deployment evidence, and completed GitHub Release.

## Intake Requirements

Release starts assembly only when:

- A GitHub Milestone is active.
- Included issues are assigned to the active Milestone.
- Included branches have passing feature QA.
- Release-impacting architecture notes are recorded for included issues when
  relevant.
- Release scope is recorded.
- The Technical Lead has requested release assembly.

## Assembling a Release

Individual feature QA is required, but it is not sufficient for production
promotion. The assembled release branch gets its own QA pass.

Release assembly rules:

- The release branch is named `release/<milestone-name>`.
- Only issues assigned to the active Milestone are eligible for the release.
- Only branches with passing feature QA are eligible for the release branch.
- Deferred issues are removed from the Milestone or moved to a future Milestone.
- The Release agent records the included issue list before release QA starts.
- Migration, deployment, compatibility, rollback, and operational notes from
  architecture review are carried into release QA and promotion notes.

Create the release branch:

```bash
cd prod/main
git fetch origin
git checkout main
git pull --ff-only origin main
git switch -c release/2026.06.1
```

For an existing release branch, update the local branch from the remote:

```bash
cd prod/main
git fetch origin
git switch release/2026.06.1
git reset --hard origin/release/2026.06.1
```

Merge approved feature branches into the release branch through the approved
GitHub process:

```bash
git merge --no-ff origin/123-checkout-banner
git push -u origin release/2026.06.1
```

## Release QA

QA creates a release test checkout:

```bash
git clone git@github.com:OWNER/REPO.git tst/release-2026.06.1
cd tst/release-2026.06.1
git switch -c release/2026.06.1 --track origin/release/2026.06.1
```

QA validates the assembled release and records release-level evidence in the
Milestone or release tracking issue.

Release does not promote or deploy from the release branch until QA has passed
release integration testing and the operator has approved the Technical Lead's
release manifest.

## Release Manifest Gate

Before deployment planning starts, Release must verify that the Technical Lead
has recorded an operator-approved release manifest. The manifest must identify:

- Included issues and deferred issues.
- Release branch and commit SHA.
- Integration QA evidence.
- Operations readiness evidence or recorded blockers.
- Deployment validation plan.
- Rollback criteria and rollback procedure reference.

After operator approval, release scope is frozen. Any additional issue requires
operator approval and an updated manifest before Release proceeds.

## Live Non-Production Smoke Gate

For platform, service, API, worker, scheduler, or other runtime milestones, a
successful test suite is not enough to call the platform operational. Before a
release is reported as complete, Release must obtain evidence that the
non-production runtime starts as real listener processes and passes live smoke
checks on its configured loopback ports.

For a new product's first runtime release, the non-production platform must be
stood up persistently as part of release completion and then kept running under
the environment ownership model. A release may report code promotion complete
without production deployment, but it must not report the non-production
platform as operational unless the persistent non-production services are
running and smoke-tested.

The live non-production smoke gate must verify, as applicable:

- service-managed entry points start successfully from the promoted artifact;
- required non-production configuration, TLS material, and secrets are resolved
  from scoped non-production locations outside Git;
- listeners bind only to approved loopback addresses and configured ports;
- liveness/readiness endpoints pass over the live listener;
- documentation endpoints load over the live listener;
- at least one representative protected operation reaches the expected
  authentication or success boundary;
- admin or operational status endpoints pass over the live listener when the
  product has them;
- logs or command output contain no secret values.

Dry-run scripts, generated route plans, unit tests, TestClient checks, and
in-process rehearsals can support this gate, but they do not replace it when
the release claims the runtime is stood up.

Generated dummy credentials, generated dummy certificates, and other ephemeral
test material may validate the smoke harness itself, but they are not acceptable
evidence that the maintained non-production platform is stood up. Operational
non-production smoke evidence must use the real host-managed non-production
credentials and TLS material approved for that environment.

If live non-production smoke cannot be performed, Release must record the
explicit reason, the operator-approved waiver, and the exact follow-up issue
that will complete the operational rollout. Without that waiver, do not report
the platform or environment as operationally complete.

Release evidence must use normal client and operator paths. Bypass flags such
as `curl -k`, ad-hoc `--cacert` when system trust is expected, browser
certificate exceptions, generated dummy credentials, generated dummy
certificates, fixture harness material, or code paths that disable certificate
verification are not acceptable release evidence.

## Staged Deployment

After the release manifest is approved, Release prepares the release plan and
deployment plan.

For products with a maintained non-production production-code environment,
Release deploys the production code there first. This environment runs the
production code path against approved non-production services, credentials,
secret roots, TLS material, service-manager ownership, and monitoring. Release
must validate deployment there before actual production deployment.

Only after non-production production-code deployment validation passes may
Release deploy to actual production. Actual production validation repeats the
required health, readiness, documentation, authentication-boundary,
service-manager, log redaction, TLS, browser-facing, and rollback-readiness
checks that apply to the product.

## Production Promotion

Release agents promote the validated release branch to production.

Expected release steps:

```bash
cd prod/main
git fetch origin
git checkout main
git pull --ff-only origin main
git merge --ff-only origin/release/2026.06.1
git push origin main
```

After production promotion, the Release agent creates the GitHub Release and
tag:

```text
Tag: v2026.06.1
Release title: 2026.06.1
Included issues: #123, #124, #125
Validation evidence: release QA run, smoke test output, deployment check
```

## Cleanup

After release, clean up completed phase checkouts:

```bash
rm -rf tst/release-2026.06.1
rm -rf tst/123-checkout-banner
rm -rf dev/123-checkout-banner
```

Delete the remote feature branch only through the approved GitHub or release
process after it has been merged.

## Handoff to Operator

Release reports:

- Release tag.
- Included issues.
- Excluded or deferred issues.
- Validation evidence.
- Production promotion result.
- Smoke check result.
