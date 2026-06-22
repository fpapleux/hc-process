# Tool Manuals

This folder contains canonical reusable tool manuals.

These manuals explain how a tool works. They do not grant authority.

Role authority is granted only by the role's `TOOLS.md`. At agent creation time,
copy only the manuals that role needs into the agent's local `tools/` folder.

Rule:

```text
Canonical tools/*.md explains usage.
Role TOOLS.md grants permission.
Role tools/*.md contains only what that role should use.
```
