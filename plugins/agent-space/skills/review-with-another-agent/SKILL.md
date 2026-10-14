---
name: review-with-another-agent
description: Use when the user wants a different Agent or model to review the current changes, for example "have Codex review this", "get a second opinion", or "ask another model to check my diff". Sends the diff to that Agent through the Agent.Space MCP server and brings its findings back.
---

# Review with another Agent

Agent.Space runs Codex, Claude Code, Grok Build, OpenCode, and DeepSeek Harness
in one Workspace, billed to the user's Agent.Space plan or credits. Every tool
below comes from the `agent-space` MCP server. The first call opens a browser
sign-in; ask the user to finish it.

The reviewer does not see the user's machine. It sees only the text you send,
so send everything it needs.

1. Collect the change. Default to `git diff HEAD` plus the content of new
   untracked files the user wants reviewed; use a branch range such as
   `git diff main...HEAD` when the user asks for it. If the diff is longer than
   about 80,000 characters, ask the user which files to include.
2. Tell the user in one line that the diff goes to Agent.Space and the review
   runs on their Agent.Space plan or credits.
3. `list_workspaces`. Use the only Workspace, or the one the user names; if
   there are several, ask which one.
4. `get_workspace`. Reuse an Agent with the harness the user asked for;
   otherwise `create_agent` with it. When the user only says "another Agent",
   pick Codex if you are Claude Code, and Claude Code if you are Codex.
5. `create_session` for that Agent.
6. `list_execution_options`. Pick an option for that harness, preferring
   `planAvailable`, then `flexAvailable`, and pass its `configuration`
   unchanged.
7. `submit_turn` with a self-contained instruction:
   - what the change is for and anything the reviewer should know;
   - "The repository is not in this Workspace. Review only the diff below. Do
     not create or modify files.";
   - what to report: bugs, security problems, and missed cases, each with the
     file, line, severity, and a suggested fix;
   - the diff in a fenced block.
8. `get_submission` with `waitSeconds: 20` until it finishes. If it returns a
   `userActionUrl`, give the link to the user; they must act there before the
   review continues.
9. Bring the result back. List the reviewer's findings, say which you agree
   with and why, and ask before changing code. Never report a review that
   `get_submission` has not returned.
