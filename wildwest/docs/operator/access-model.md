# Wildwest Access Model

Wildwest should explain file access in operator language, not raw Unix jargon.

## Simple Rule

An agent can do exactly what its Linux identity is allowed to do.

If an agent is added to a powerful group, it gets that group's power. There is no special "agent" restriction at the filesystem layer.

## Access Levels

Wildwest should expose simple access levels:

| Level | Meaning | Typical Unix/ACL permissions |
| --- | --- | --- |
| `none` | Cannot access the path | no permissions |
| `read` | Can list and read files | directories `r-x`, files `r--` |
| `run` | Can read and execute/traverse, but not modify | directories `r-x`, executable files `r-x`, regular files `r--` |
| `write` | Can create, edit, delete, and rename | directories `rwx`, files `rw-` |
| `admin` | Can manage files and access policy | `rwx` plus Wildwest-managed ownership/ACL authority |

For most operators:

- use `read` when the agent only needs to inspect
- use `run` when the agent needs to execute project code but not change it
- use `write` when the agent may modify the project
- avoid `admin` unless the agent is explicitly trusted to manage the area

## Groups Are Broad

Putting an agent in a group grants all matching group permissions.

If `/opt/strapi` is writable by `admins`, then an agent in `admins` can write there too.

For safer setups, create narrower groups:

```text
strapi-admins       humans who can modify production Strapi
ww-strapi-runner    agents that can read/run production Strapi
ww-strapi-writer    agents that can modify selected Strapi paths
```

## When ACLs Are Needed

Classic Unix permissions allow one owner and one group. That is enough for simple cases.

Use ACLs when the same folder needs different permissions for multiple groups:

```text
fabien: rwx
strapi-admins: rwx
ww-strapi-runner: r-x
ww-strapi-writer: rwx on selected subfolders only
others: none
```

Wildwest should hide most `setfacl` complexity behind commands like:

```bash
wildwest grant ww-strapi-runner /opt/strapi --access run
wildwest grant ww-strapi-writer /opt/strapi/uploads --access write
```

## Inheritance

Access on a directory is not enough. New files and subdirectories must inherit the same policy.

Wildwest should set both:

- current ACLs for files that already exist
- default ACLs on directories for files created later

## Important Warning

Read/run access is not always harmless.

If an agent can read `.env`, SSH keys, API tokens, database dumps, or production config, then the agent can use or leak those secrets. Wildwest should eventually support secret-aware grants and warnings.
