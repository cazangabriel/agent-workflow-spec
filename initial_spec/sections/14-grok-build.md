### 14.7 Human-guide files (how-to-for-human)

Once a project is started (Phase 1 begin), markdown files are generated for the human only. They are not living-knowledge for agents. They are a short operating manual for the person who spawns and talks to the agents.

Paths:

```text
/app-name-1/human-guides/DesignArchitect-warm.md
/app-name-1/human-guides/KanbanMaster-cold.md
/app-name-1/human-guides/Worker-cold.md
/app-name-1/human-guides/glossary.md
```

Each role file must answer, in plain language:

- What this role is for (and what it is not for).
- Warm or cold: whether to resume the same session or start a new one each time.
- When to start it (Phase 1, ceremony, ticket assignment, blocker, etc.).
- How to start it (separate terminal, `cd` into the shared checkout, `grok --no-memory`, new session, `/rename`).
- What it is allowed to write, and what it must never write.
- The session-start worktree check and the commit-and-push proof it must give you.
- How you interact with it (approach approval, test loop, escalation, Phase 1 exit question).
- One concrete example of a good first message and a bad first message.

These files are generated from this workflow spec (not invented per project). They may be regenerated if the workflow spec changes, but they are not the place for project-specific architecture. Agents must not treat them as living-knowledge.

### 14.8 Glossary (human-readable)

Generated at `/app-name-1/human-guides/glossary.md`. Written so the human can understand the workflow without returning to the design conversation. Agents do not treat it as living-knowledge.

Required entries (plain language):

- **Friction.** A number on a ticket that starts at 0. Each time something fails — a test fails, you send the ticket back, or the worker hits the stop-rule — the worker adds 1 and writes a short comment saying why. There is no upper limit and it is not a difficulty rating the agent invents. A high number means the ticket has bounced a lot. Look at it on `kanban.md` when you want to see which tickets have been painful.
- **Living-knowledge.** The markdown files under `/app-name-1/living-knowledge/` that hold how this product is actually built: module map, conventions, gotchas, abbreviation lists. Agents read them so they do not guess. Workers do not write them. This is the source of truth, not Grok Build Memory and not session chat.
- **Design spec.** The Phase 1 document that says what the product is, which modules exist, which technologies were chosen, and in what order components should be built. Only DesignArchitect-warm and you write it. KanbanMaster-cold reads it to cut tickets.
- **F ticket.** A feature / implementation ticket for Worker-cold. One thing you can test, plus one approach discussion.
- **TA ticket.** A technical-architecture ticket for DesignArchitect-warm. It states a design problem (impact and what is needed). It does not contain the solution. Only KanbanMaster-cold creates them.
- **Warm.** DesignArchitect-warm. Resume the same session when you can, so design talk stays in one place. After a reset it re-reads the design spec and living-knowledge.
- **Cold.** KanbanMaster-cold and Worker-cold. Start a new session each ceremony or assignment. Do not resume an old thread as if it were still cheap.
- **Phase 1.** Design work until the design spec is good enough to cut tickets from. You explicitly accept Phase 1 done.
- **Phase 2.** Ticketed delivery against that accepted spec. Ceremonies cannot be skipped.
- **Mini Phase 1.** A new product area with its own language was added that the current design spec does not cover. Send DesignArchitect-warm back to update the spec for that slice. A normal new feature inside an already-mapped module is not Mini Phase 1.
- **Stop-rule.** The worker stops when it is not sure how to proceed, hits something living-knowledge does not cover, or the work looks like a design change. It comments, tells you, and does not invent the missing rule.
- **Escalate band.** A rough “ask first” signal for DesignArchitect-warm and KanbanMaster-cold. If a design change looks like it would burn a large share of the week’s tokens, they stop and ask you before doing it. Worker tickets that look that large are split instead.
- **Column folders.** Ticket state is the folder the file sits in (`open`, `in-progress`, `in-testing`, `blocked`, `done`, `closed`, `closed-wont-do`, `backlog`). The filename does not contain the state.
- **NEXT.** The file `/app-name-1/tickets/NEXT` that holds the next ticket number as a single integer. Only KanbanMaster-cold updates it. Nobody scans hundreds of filenames to find the next number.
- **Worktree.** An isolated second checkout Grok Build can create. This workflow does not use them. All three roles work on the shared `dev` tree so you can see ticket moves and living-knowledge edits immediately.
- **Grok Build Memory.** An optional automatic note-taking feature in Grok Build. Disabled for this workflow. Living-knowledge is the designed memory.
- **Kanban ceremony.** A fresh KanbanMaster-cold session that pulls tickets from backlog into open, reviews blockers and friction, closes accepted done tickets, and regenerates `kanban.md`. Without it, no new backlog tickets can legally appear.
