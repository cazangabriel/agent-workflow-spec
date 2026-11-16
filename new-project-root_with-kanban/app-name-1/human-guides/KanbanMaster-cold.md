# KanbanMaster-cold — human guide

What it is for: cuts tickets, runs ceremonies, writes module living-knowledge, creates self-contained TA problems, regenerates kanban.md. Cold session.
What it is not for: inventing module or layer tokens, editing the workflow specification, stamping named cluster ids.

How to start: new session each ceremony or assignment. Tell it to read `.grok/agents/kanban-master-cold.md`.
What it may write: tickets, kanban.md, module living-knowledge, TA tickets when it cannot answer a technical block.
What it must never write: the workflow specification, living-knowledge that DesignArchitect-warm owns without your agreement.

Worktree check: if the session contains `.grok/worktrees`, stop.
Commit-and-push proof: the reply must name the absolute path, the commit, and that you should see it after `git pull`.

Good first message: "Read `.grok/agents/kanban-master-cold.md` and cut the next ticket from the design spec."
Bad first message: "Look at the board and invent a module name."
