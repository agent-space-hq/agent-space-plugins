---
name: review
description: Use when the user wants another AI agent, or several, to review the current code change, for example "have Codex review this", "get a second opinion on my diff", "have Codex, Grok and DeepSeek review this", "multi-model code review", or "panel review". Each agent reviews the diff independently through Agent.Space; the findings come back merged, with the ones several agents found first.
argument-hint: "[base branch] [with codex, grok, deepseek, opencode, claude code]"
---

# Code review by other agents

Agent.Space runs Codex, Claude Code, Grok Build, OpenCode, and DeepSeek
Harness side by side in one shared Workspace, billed to the user's
Agent.Space plan or credits. This skill has one or more of them review the
same diff independently, then merges what they found. Every tool below comes
from the `agent-space` MCP server. The first call opens a browser sign-in;
ask the user to finish it.

Arguments: $ARGUMENTS

1. Collect the change. Default to `git diff HEAD` plus the content of new
   untracked files the user wants reviewed. When the arguments name a base
   branch, use `git diff <base>...HEAD`. If the diff is longer than about
   60,000 characters, ask the user which files to include.
2. Pick the panel. Use the agents the user names. When the user asks for "a
   second opinion" or "another agent", pick one: Codex when you are Claude
   Code, Claude Code when you are Codex. Otherwise pick three from different
   model makers, leaving out the harness you are running in. Harness
   IDs: `codex` (Codex), `grok` (Grok Build), `deepseekHarness` (DeepSeek
   Harness), `openCode` (OpenCode), `claudeCode` (Claude Code). When you are
   Claude Code, the default panel is `codex`, `grok`, `deepseekHarness`.
3. Tell the user in one line which agents will review, that the diff goes to
   Agent.Space, and that the reviews use their Agent.Space plan or credits.
4. `list_workspaces`. Use the only Workspace, or the one the user names; if
   there are several, ask which one.
5. `get_workspace`. For each harness on the panel, reuse an Agent with that
   harness, or `create_agent` with it.
6. `list_execution_options` once. For each Agent, pick an option for its
   harness, preferring `planAvailable`, then `flexAvailable`, and pass its
   `configuration` unchanged.
7. For each Agent, `create_session`, then `submit_turn` with the same
   self-contained instruction. Start every Agent before you wait on any of
   them. The instruction contains:
   - what the change is for and anything the reviewer should know;
   - "The repository is not in this Workspace. Review only the diff below. Do
     not create or modify files.";
   - what to report: bugs, security problems, and missed cases, each with the
     file, line, severity (high, medium, or low), and a suggested fix, or "No
     issues found";
   - the diff in a fenced block.
8. Call `get_submission` with `waitSeconds: 20` on each unfinished submission
   in turn until all have finished. If one returns a `userActionUrl`, give the
   link to the user. If one fails, or is still running after about ten
   minutes, go on without it and say so.
9. Merge the findings. Treat reports of the same problem as one finding.
   Order by how many agents found it, then by severity, in one table:
   finding, file and line, severity, found by, and your verdict (agree or
   disagree, with a one-line reason). Then give a one-line overall verdict on
   the change.

   Never report a review that `get_submission` did not return. Ask before
   changing any code. End with one line saying the full reviews are in the
   user's Agent.Space Workspace, where teammates can open them.
