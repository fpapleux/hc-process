# Hearts & Colors Python Overlay

Use this overlay only when a role and product profile explicitly authorize
Hearts & Colors Python work.

These rules specialize the generic Python coding operating model for Hearts &
Colors, CMS, Shopify, OpenClaw, and company automation scripts. They are not
generic Python advice.

## Operating Direction

Future Hearts & Colors Python work uses:

- role-scoped skills;
- product-scoped configuration;
- environment-specific credentials;
- explicit local/test/production lanes;
- approved MCP/tool servers for sensitive operations.

Do not write new code that assumes blanket access to the operator vault.

## Operational Boundaries

- Do not write to databases, CMS, Shopify, mailboxes, files of record, or other
  external state without explicit role authority for that operation.
- Do not share private, proprietary, customer, credential, or company data
  outside the local system unless specifically approved.
- During diagnosis, do not make changes. Identify root cause first, gather
  supporting evidence, then propose or execute the justified fix.
- Do not use workarounds without explicit approval from the operator.
- Do not claim work is complete unless it was executed and verified.

## OpenClaw Skill Layout

For Hearts & Colors OpenClaw skills, use this layout:

```text
<skill-name>/
    SKILL.md
    scripts/
        task_script.py
    references/        # only when detailed docs are needed
    assets/            # only when output templates/assets are needed
```

Keep `SKILL.md` as the procedural interface. Keep deterministic operations in
`scripts/`. Do not add README, changelog, or unrelated support files inside a
skill unless they are directly consumed by the skill.

## Configuration and Secrets

Current legacy scripts may still load:

```text
Path(os.environ["OPENCLAW_STATE_DIR"]) / "environment.local"
```

Treat that as a legacy compatibility path, not as the standard for new work.

New or substantially changed scripts use scoped configuration:

```text
developer role -> local/dev credentials only
QA role        -> test/staging credentials only
release role   -> production credentials through approved release tooling
```

Rules:

- Do not hard-code credentials, tokens, domains, production paths, Shopify
  identifiers, CMS URLs, or mailbox settings in scripts.
- Do not read the operator vault from ordinary agent code.
- Load only the scoped configuration assigned to the concrete role/product.
- Validate mandatory configuration before side effects.
- Fail closed when configuration is missing or ambiguous.

## Environment Targeting

Do not add casual `--prod` switches.

For mutable external systems, use explicit environment lanes:

```text
local/dev -> test/staging -> production
```

The target environment comes from role-scoped configuration and product process,
not from developer convenience flags.

CMS example:

- Developer scripts target local/dev CMS.
- QA scripts target test/staging CMS.
- Release/operations tooling targets production CMS only after approval gates.

## Hearts & Colors Script Pattern

Architect every script so an agent can either import it or run it from the
shell.

Preserve existing local conventions when changing existing files. For new
scripts, prefer:

- importable domain logic;
- thin CLI adapter;
- scoped configuration passed into dependencies;
- JSON stdout for agent-consumed output;
- stderr for diagnostics;
- dry-run or preview behavior for batch, file, Shopify, CMS, or destructive
  operations.

Existing CMS scripts may use inline argparse inside
`if __name__ == "__main__":`. Preserve that convention when editing those files
unless a scoped refactor explicitly changes the pattern.

## CMS and Business-Operation Scripts

For Hearts & Colors CMS/operations scripts:

1. Read only the role/product/technology instructions assigned to the agent.
2. Query current state first.
3. Prepare payloads in temp or staging files.
4. Run against the role-authorized non-production environment first.
5. Verify state by querying it back.
6. Production execution belongs to release/operations tooling after approval
   gates.
7. Verify production by querying it back when production execution is authorized.
8. Report exact IDs, docnames, locales, files, and counts changed.
9. Remove temporary artifacts unless they are needed for audit.

## Verification Standard

Before reporting completion for Hearts & Colors scripts, include:

- command(s) run;
- target environment;
- changed records/files/IDs/counts;
- verification command(s) run;
- unresolved risk or follow-up, if any.
