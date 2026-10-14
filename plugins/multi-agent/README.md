# Multi-Agent for Claude Code

Ask Codex, Grok, DeepSeek, OpenCode, and Claude Code the same question in
parallel, or have them all review your diff, without leaving Claude Code. You
get one answer back that shows where the agents agree, where they disagree,
and what only one of them caught.

The agents run in your [Agent.Space](https://agent.space) Workspace, a
multiplayer, multi-agent workspace, so you don't install their CLIs or manage
API keys, and your teammates can open every answer.

## Install

```
/plugin marketplace add agent-space-hq/agent-space-plugins
/plugin install multi-agent@agent-space
```

This also installs the `agent-space` plugin, which connects Claude Code to
Agent.Space. The first run opens a browser sign-in.

## Commands

| Command | What it does |
| --- | --- |
| `/multi-agent:ask <question>` | Sends the question to three agents from different model makers at once and compares their answers. Ask for a second round and each agent critiques the others' answers without knowing who wrote them. |
| `/multi-agent:review [base branch]` | Each agent reviews your diff independently. Findings come back merged and ranked by how many agents found them, with Claude Code's verdict on each. |

Name the agents you want, for example
`/multi-agent:ask with codex and grok: is this migration safe to run online?`.
By default Claude Code asks Codex, Grok Build, and DeepSeek Harness.

You can also just say it: "ask a few other models whether this cache key is
right" or "have Codex, Grok and DeepSeek review my changes".

## What is sent, and what it costs

Only what Claude Code puts in the prompt: your question, the diff, and the
context it chooses to include. The agents cannot read your machine. Each run
uses your Agent.Space plan or credits; Claude Code tells you which agents it
is about to run before it starts.

## How is this different from running other CLIs from Claude Code?

Plugins that call other agents locally need each CLI installed and signed in
on your machine, and each one talks to a single model maker. Here one
Agent.Space account covers Codex, Claude Code, Grok Build, OpenCode, and
DeepSeek Harness, the runs happen in the cloud, and the results stay in a
Workspace your team shares.

## Links

- [Agent.Space](https://agent.space)
- [All Agent.Space plugins](https://github.com/agent-space-hq/agent-space-plugins)
- Support: support@agent.space
