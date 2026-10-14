# Agent.Space plugins

Ask another Agent for a second opinion without leaving Claude Code: have Codex
review what Claude Code just wrote, or the reverse. The review runs in
[Agent.Space](https://agent.space), a multiplayer, multi-agent workspace for
Codex, Claude Code, Grok Build, OpenCode, and DeepSeek Harness, on your
Agent.Space plan or credits.

## Claude Code

```
/plugin marketplace add agent-space-hq/agent-space-plugins
/plugin install agent-space@agent-space
```

Then ask: "Have Codex review my changes." The first run opens a browser
sign-in to Agent.Space.

To bring a teammate into the same Workspace, ask: "Get me an invite link for
this workspace." Claude Code gives the link to you; it never sends it to anyone.

## Any MCP client

The plugin is a skill on top of the Agent.Space MCP server. Any client that
supports remote MCP servers with OAuth can connect to it directly at
`https://agent.space/mcp`. In Claude Code without the plugin:

```
claude mcp add --transport http agent-space https://agent.space/mcp
```

## What is sent

Only what the calling Agent puts in the request: the diff and its context. The
reviewer cannot read your machine. Agent.Space keeps the review in your
Workspace, where you and your teammates can open it.
