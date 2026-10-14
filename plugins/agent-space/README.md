# Agent.Space plugin for Claude Code

Bring Codex, Grok, DeepSeek, OpenCode, and your teammates into Claude Code.
[Agent.Space](https://agent.space) is a multiplayer, multi-agent workspace:
many agents, many humans, one space. This plugin lets Claude Code ask other
agents for answers and reviews, and lets your team follow your Claude Code
sessions live, in one shared Workspace.

```
/plugin marketplace add agent-space-hq/agent-space-plugins
/plugin install agent-space@agent-space
```

The first run opens a browser sign-in to Agent.Space.

## Commands

| Command | What it does |
| --- | --- |
| `/agent-space:ask <question>` | Sends the question to three agents from different model makers at once and compares their answers. |
| `/agent-space:review [base branch]` | Each agent reviews your diff independently. Findings come back merged, with Claude Code's verdict on each. |
| `/agent-space:share [workspace]` | Shares this conversation into your Agent.Space Workspace, so teammates can follow it live and read-only. It updates after every turn; `/agent-space:share stop` removes it. |

Name the agents you want, or let Claude Code pick Codex, Grok Build, and
DeepSeek Harness. You can also just say it: "have Codex review my changes" or
"ask a few other models whether this cache key is right".

## Examples

```
/agent-space:ask Should the job queue use Postgres advisory locks or a Redis lock?
/agent-space:ask with codex and grok: is this migration safe to run while the app is live?
/agent-space:review main
/agent-space:share
```

`ask` returns the best answer, a table of where the agents agree and differ,
and anything only one agent raised. Ask for a second round and each agent
critiques the others' answers without knowing who wrote them. `review`
returns one list of findings, ranked by how many agents found each one, with
Claude Code's verdict on each. `share` gives you a link to the conversation in
your Workspace, where teammates see your prompts, Claude Code's replies, and
its plan next to everyone else's sessions and agents.

## Why Agent.Space

- **No CLIs or API keys.** The agents run in your Agent.Space Workspace. One
  account covers Codex, Claude Code, Grok Build, OpenCode, and DeepSeek
  Harness.
- **Different model makers, side by side.** A second opinion from a model
  trained by someone else catches different mistakes.
- **Your team sees it.** Every answer and review stays in a shared Workspace,
  where teammates and their agents can pick it up.
- **One bill.** Runs use your Agent.Space plan or credits.

## What is sent

Only what Claude Code puts in the request: the question or diff and the
context it chooses to include. Claude Code tells you which agents it is about
to run before it sends anything. The agents cannot read your machine.

The plugin talks only to the Agent.Space MCP server at
`https://agent.space/mcp`. Requests and results are stored in your Workspace,
where its members can see them.

When you share a conversation, each turn sends your prompt, Claude Code's
reply, and its plan. Tool output, file contents, and commands stay on your
computer, and anything that looks like a secret is replaced with
`[redacted]`. Every member of the Workspace, including viewers, can read it.

## Use it from any MCP client

The plugin is skills on top of the Agent.Space remote MCP server at
`https://agent.space/mcp` (Streamable HTTP, OAuth). Any client that supports
remote MCP servers can connect to it. In Claude Code without the plugin:

```
claude mcp add --transport http agent-space https://agent.space/mcp
```

## FAQ

**How is this different from OpenAI's Codex plugin for Claude Code?**
OpenAI's plugin runs the Codex CLI installed on your machine, signed in with
ChatGPT or an OpenAI API key, and talks only to Codex. Agent.Space runs agents
from several model makers in the cloud, with nothing to install, and keeps the
results in a Workspace your team shares. You can use both.

**How is sharing different from Claude Code's Remote Control?**
Remote Control lets you continue your own local session from your phone or
the browser. `/agent-space:share` is for your team: everyone in the Workspace
can follow your session as it runs on your computer, next to their own
sessions and agents.

**Is Agent.Space the same as Google Agentspace?**
No. Agent.Space ([agent.space](https://agent.space)) is an independent product
and is not affiliated with Google.

**What does it cost?**
Each run uses your Agent.Space plan or credits. See
[agent.space](https://agent.space) for plans and model prices.

## Support

Open an [issue](https://github.com/agent-space-hq/agent-space-plugins/issues)
or email support@agent.space. MIT licensed.
