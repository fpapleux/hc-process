# 0009 Use ACLs For Multi-Group File Access

## Decision

Wildwest should treat Linux ACLs as a first-class file-access mechanism.

Classic Unix permissions remain the baseline, but ACLs are required when different users or groups need different permissions on the same path.

## Reason

Classic Unix mode bits provide only one owner, one group, and one "others" permission set. That is not enough for common Wildwest cases, such as:

- human admins can modify `/opt/strapi`
- an agent can read and execute files under `/opt/strapi`
- another agent can write only to one runtime directory
- everyone else has no access

Linux ACLs allow Wildwest to express those grants without awkward ownership workarounds.

## Consequences

- Wildwest should present simple operator-facing access levels instead of exposing raw ACL syntax first.
- Internally, Wildwest can map those access levels to `setfacl` and default ACLs.
- Wildwest should explain when classic group permissions are enough and when ACLs are needed.
- ACL inheritance matters: directories need default ACLs so new files keep the intended access policy.

## Open Questions

- What should the first grant command look like?
- Should Wildwest store intended grants in its own state and reconcile ACLs, or only apply them once?
- How should Wildwest detect filesystems that do not support ACLs?
- Should access grants target users, groups, agents, or named roles?

