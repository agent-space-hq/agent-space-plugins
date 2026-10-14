---
name: invite-a-teammate
description: Use when the user wants to bring a teammate into their Agent.Space Workspace, for example "invite Alex to this workspace", "get me an invite link", or "let my teammate see this review". Creates the link through the Agent.Space MCP server and gives it to the user.
---

# Invite a teammate

An Agent.Space Workspace is shared: teammates who join see its Agents and
sessions and can run their own Agents next to yours. Every tool below comes
from the `agent-space` MCP server. The first call opens a browser sign-in; ask
the user to finish it.

1. `list_workspaces`. Use the only Workspace, or the one the user names; if
   there are several, ask which one.
2. `create_invite` with that Workspace. Use the `editor` role, which lets the
   teammate run Agents; use `viewer` when the user only wants them to watch.
   Only the Workspace owner can create a link; if the call is refused for that
   reason, tell the user to ask the owner.
3. Give the user `inviteUrl` and `expiresAt`. Anyone with the link can join
   until it expires, so never send, post, or share it anywhere yourself: the
   user decides who gets it.
