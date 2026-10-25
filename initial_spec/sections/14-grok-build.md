### 14.1 What Grok Build gives us (facts, not wishes)

- A session is one conversation with its own context. You start it (`grok` in a repo), resume it, or start `/new`.
- The dashboard lists sessions. You dispatch new ones. That is how the human spawns the roles.
- Subagents are child sessions the agent can spawn. Isolation is `none` (shared working tree, default) or `worktree` (isolated git checkout under `~/.grok/worktrees/...`).
- Worktrees exist so parallel editors do not overwrite each other. They are a second working copy, not `dev`.
- `AGENTS.md` and `.grok/agents/` load every session in that tree.
- Skills, plugins, and MCP exist. They do not replace living-knowledge.
- Plan mode exists. It can host the approach-discussion step. It does not replace human approval.
- Memory is an optional automatic markdown cache. It is off by default. It is a generic container. It does not implement the living-knowledge rules in this spec.

### 14.7 Human-guide files

Once a project is started, markdown files are generated for the human only. They are not living-knowledge for agents.

```text
/app-name-1/human-guides/DesignArchitect-warm.md
/app-name-1/human-guides/KanbanMaster-cold.md
/app-name-1/human-guides/Worker-cold.md
/app-name-1/human-guides/glossary.md
```

Each role file must answer, in plain language: what the role is for and not for; warm or cold; when to start it; how to start it (`cd` into the shared checkout, `grok --no-memory`, new session, first message that names the init file); what it may write and must never write; the session-start worktree check and the commit-and-push proof; how you interact with it; one good first message and one bad first message.

These files are generated from this workflow spec. Agents must not treat them as living-knowledge. Init files do not depend on them.
Violation: an init file that says "read the human guide."
Example: the init file lists the spec sections it must obey. It does not point at `human-guides/Worker-cold.md`.

### 14.8 Glossary (human-readable)

Generated at `/app-name-1/human-guides/glossary.md`. Written so the human can understand the workflow without returning to the design conversation. Agents do not treat it as living-knowledge.

- Friction. A number on a ticket that starts at 0. Each time something fails — a test fails, you send the ticket back, or the worker hits the stop-rule — the worker adds 1 and writes a short comment saying why. There is no upper limit and it is not a difficulty rating. A high number means the ticket has bounced.
- Living-knowledge. The markdown files under `/app-name-1/living-knowledge/` that hold how this product is actually built. Agents read them so they do not guess. Workers do not write them. This is the source of truth, not Grok Build Memory and not session chat.
- Design spec. The Phase 1 document. Only DesignArchitect-warm and you write it. KanbanMaster-cold reads it to cut tickets.
- F ticket. A feature ticket for Worker-cold. One thing you can test, plus one approach discussion.
- TA ticket. A technical-architecture ticket for DesignArchitect-warm. It states a design problem. It does not contain the solution. Only KanbanMaster-cold creates them.
- Warm. DesignArchitect-warm. Resume the same session when you can.
- Cold. KanbanMaster-cold and Worker-cold. Start a new session each ceremony or assignment.
- Phase 1. Design work until you explicitly accept the design spec as done.
- Phase 2. Ticketed delivery against that accepted spec.
- Mini Phase 1. A new product area whose language the current design spec does not cover. A normal new feature inside an already-mapped module is not Mini Phase 1.
- Stop-rule. The worker stops when it is not sure, hits something living-knowledge does not cover, or the work looks like a design change. It comments, tells you, and does not invent the missing rule.
- Escalate band. A rough ask-first signal for DesignArchitect-warm and KanbanMaster-cold. Worker tickets that look that large are split instead.
- Column folders. Ticket state is the folder the file sits in. The filename does not contain the state.
- NEXT. `/app-name-1/tickets/NEXT` holds the next ticket number as a single integer. Only KanbanMaster-cold updates it.
- Worktree. An isolated second checkout. This workflow does not use them.
- Grok Build Memory. An optional automatic note-taking feature. Disabled for this workflow.
- Kanban ceremony. A fresh KanbanMaster-cold session that pulls tickets, reviews blockers, closes accepted done tickets, and regenerates `kanban.md`.
