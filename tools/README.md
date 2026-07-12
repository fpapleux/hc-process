# Tool Manuals

This folder contains canonical reusable tool manuals.

These manuals explain how a tool works. They do not grant authority.

Role authority is granted only by the role's `TOOLS.md`. At agent creation time,
the role profile references only the canonical manuals that role needs.

Rule:

```text
Canonical tools/*.md explains usage.
Role TOOLS.md grants permission.
Role-specific supplements contain only unique role-specific content.
Do not copy canonical manuals into role folders.
```
