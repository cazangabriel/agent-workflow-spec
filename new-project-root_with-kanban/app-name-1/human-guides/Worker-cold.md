# Worker-cold — human guide

What it is for: implements assigned tickets. Cold session.
What it is not for: writing living-knowledge, the design spec, tickets, or the workflow specification.

How to start: new session each assignment. Tell it to read `.grok/agents/worker-cold.md`.
What it may write: code, commits, ticket comments. It moves tickets only among open / in-progress / in-testing / blocked. It moves in-testing to done only after you signal the ticket is done.
What it must never write: living-knowledge, tickets, the design spec.

Worktree check: if the session contains `.grok/worktrees`, stop.
Commit-and-push proof: the reply must name the absolute path, the commit, and that you should see it after `git pull`.

Good first message: "Read `.grok/agents/worker-cold.md` and implement the ticket I just assigned."
Bad first message: "Create a living-knowledge file for the missing unit."
