---
name: share
description: Use when the user wants teammates to see this Claude Code conversation or follow its progress, for example "share this session with my team", "let my team follow along", "sync this conversation to Agent.Space", or "stop sharing". Shares the conversation into an Agent.Space Workspace, where members follow it live and read-only, and updates it after every turn.
argument-hint: "[workspace name] | stop"
---

# Share this conversation with your team

Agent.Space is a multiplayer, multi-agent workspace. This skill shares the
conversation you are in into an Agent.Space Workspace, where every member,
including viewers, can follow it read-only next to the team's other
conversations and agents. Every tool below comes from the `agent-space` MCP
server. The first call opens a browser sign-in; ask the user to finish it.

Arguments: $ARGUMENTS

This conversation's ID: ${CLAUDE_SESSION_ID}

## What is sent

Each exchange is the user's prompt, your final reply to it, and your plan if
you keep one. Never send tool output, file contents, diffs, or full commands;
say in words what you did instead. Before sending, replace anything that
looks like a secret, such as an API key, token, password, private key, or a
URL with credentials, with `[redacted]`.

## Start sharing

1. Tell the user in one line: "Sharing this conversation to Agent.Space:
   every member of the Workspace can read your prompts, my replies, and my
   plan. Tool output, file contents, and commands stay on your computer."
2. `list_workspaces`. Use the Workspace the arguments name, or the only one;
   if there are several, ask which one.
3. `share_session` with:
   - `workspaceId`;
   - `harness`: `claudeCode`;
   - `sourceSessionId`: this conversation's ID above;
   - `title`: a short title for the conversation, under 120 characters;
   - `branch`: the current git branch, if you know it;
   - `turns`: the latest exchanges of this conversation, oldest first, at
     most 20. Leave out this request to share.
4. Give the user the returned `sessionUrl` and say teammates can open it to
   follow along. Teammates who are not members yet need an invite from the
   Workspace's Members page.

## While sharing

At the end of every later turn in which you answer the user, call
`share_session` once more with the same `workspaceId`, `harness`, and
`sourceSessionId`, and `turns` holding just that exchange. Do this without
mentioning it. An exchange sent twice is recorded once, so if you are unsure
whether one was sent, send it. If a call fails twice in a row, tell the user
and stop sending.

## Stop sharing

When the arguments say stop, or the user asks to stop sharing, call
`share_session` with the same `workspaceId`, `harness`, and `sourceSessionId`,
`removed: true`, and `turns: []`. Tell the user the conversation was removed
from the Workspace, and send no more updates.
