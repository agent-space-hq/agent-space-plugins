---
name: ask
description: Use when the user wants several AI coding agents to answer the same question or weigh in on the same decision, for example "ask Codex, Grok and DeepSeek", "what do other models think", "get a few opinions on this", "run an LLM council", or "compare agents on this". Sends the question to several agents in parallel through Agent.Space and returns one answer that shows where they agree and disagree.
argument-hint: "[question] [with codex, grok, deepseek, opencode, claude code]"
---

# Ask several agents at once

Agent.Space runs Codex, Claude Code, Grok Build, OpenCode, and DeepSeek
Harness side by side in one shared Workspace, billed to the user's
Agent.Space plan or credits. This skill sends one question to several of them
in parallel and compares the answers. Every tool below comes from the
`agent-space` MCP server, which the `agent-space` plugin provides. The first
call opens a browser sign-in; ask the user to finish it.

Question: $ARGUMENTS

If the question is empty, use what the user just asked.

1. Pick the panel. Use the agents the user names. Otherwise pick three from
   different model makers, leaving out the harness you are running in. Harness
   IDs: `codex` (Codex), `grok` (Grok Build), `deepseekHarness` (DeepSeek
   Harness), `openCode` (OpenCode), `claudeCode` (Claude Code). When you are
   Claude Code, the default panel is `codex`, `grok`, `deepseekHarness`.
2. Write one self-contained prompt. The agents cannot see the user's machine
   or this conversation, so include the question, the goal and constraints,
   and the code, errors, or file excerpts that matter. Keep it under about
   60,000 characters; if it would be longer, ask the user what to leave out.
   End it with: "Answer independently and concisely. Do not create or modify
   files. Say how confident you are and what would change your answer."
3. Tell the user in one line which agents will answer, that the prompt goes to
   Agent.Space, and that the runs use their Agent.Space plan or credits.
4. `list_workspaces`. Use the only Workspace, or the one the user names; if
   there are several, ask which one.
5. `get_workspace`. For each harness on the panel, reuse an Agent with that
   harness, or `create_agent` with it.
6. `list_execution_options` once. For each Agent, pick an option for its
   harness, preferring `planAvailable`, then `flexAvailable`, and pass its
   `configuration` unchanged.
7. For each Agent, `create_session`, then `submit_turn` with the same prompt.
   Start every Agent before you wait on any of them.
8. Call `get_submission` with `waitSeconds: 20` on each unfinished submission
   in turn until all have finished. If one returns a `userActionUrl`, give the
   link to the user. If one fails, or is still running after about ten
   minutes, go on without it and say so.
9. Report, naming the agent behind every point:
   - **Answer**: the best answer in a few sentences, in your own judgment, and
     why.
   - **Where they agree** and **where they differ**: one short table, a row
     per point and a column per agent.
   - Anything only one agent raised that matters.

   Never report an answer that `get_submission` did not return, and never
   present your own view as an agent's. End with one line saying the full
   answers are in the user's Agent.Space Workspace, where teammates can open
   them.
10. If the user asks for a second round, send each Agent the other answers,
    labeled Answer A, B, and C without names, and ask it to point out errors
    and say which answer is best. Then report again the same way.
