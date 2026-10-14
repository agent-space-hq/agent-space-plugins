# Agent.Space plugins for Claude Code

Bring Codex, Grok, DeepSeek, OpenCode, and your teammates into Claude Code.
[Agent.Space](https://agent.space) is a multiplayer, multi-agent workspace:
many agents, many humans, one space. These plugins let Claude Code ask other
agents for answers and reviews, and keep the results where your team can see
them.

```
/plugin marketplace add agent-space-hq/agent-space-plugins
/plugin install multi-agent@agent-space
```

The first run opens a browser sign-in to Agent.Space.

## Plugins

| Plugin | What it does | Use it |
| --- | --- | --- |
| [`multi-agent`](plugins/multi-agent) | Ask Codex, Grok, DeepSeek, OpenCode, and Claude Code the same question in parallel, or have them all review your diff. You get one answer that shows where they agree and disagree. | `/multi-agent:ask`, `/multi-agent:review` |
| [`agent-space`](plugins/agent-space) | Connects Claude Code to Agent.Space, asks one other agent for a second opinion, such as Codex reviewing what Claude Code just wrote, and gets you an invite link so a teammate can join your Workspace. `multi-agent` installs it for you. | "Have Codex review my changes", "Get me an invite link for this workspace" |

## Examples

```
/multi-agent:ask Should the job queue use Postgres advisory locks or a Redis lock?
/multi-agent:ask with codex and grok: is this migration safe to run while the app is live?
/multi-agent:review main
```

`ask` returns the best answer, a table of where the agents agree and differ,
and anything only one agent raised. Ask for a second round and each agent
critiques the others' answers without knowing who wrote them. `review`
returns one list of findings, ranked by how many agents found each one, with
Claude Code's verdict on each.

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

## Use it from any MCP client

The plugins are skills on top of the Agent.Space remote MCP server at
`https://agent.space/mcp` (Streamable HTTP, OAuth). Any client that supports
remote MCP servers can connect to it. In Claude Code without the plugins:

```
claude mcp add --transport http agent-space https://agent.space/mcp
```

## FAQ

**How is this different from OpenAI's Codex plugin for Claude Code?**
OpenAI's plugin runs the Codex CLI installed on your machine, signed in with
ChatGPT or an OpenAI API key, and talks only to Codex. Agent.Space runs agents
from several model makers in the cloud, with nothing to install, and keeps the
results in a Workspace your team shares. You can use both.

**Is Agent.Space the same as Google Agentspace?**
No. Agent.Space ([agent.space](https://agent.space)) is an independent product
and is not affiliated with Google.

**What does it cost?**
Each run uses your Agent.Space plan or credits. See
[agent.space](https://agent.space) for plans and model prices.

## Support

Open an [issue](https://github.com/agent-space-hq/agent-space-plugins/issues)
or email support@agent.space. MIT licensed.
