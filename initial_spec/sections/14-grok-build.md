## 14. Grok Build platform constraints

This section maps the workflow onto Grok Build as it exists at the time of writing (sessions, dashboard, subagents, worktrees, AGENTS.md, skills, Memory). Platform details can change. If a later Grok Build release contradicts a rule here, raise it — do not silently follow the platform against this spec.

### 14.1 What Grok Build gives us (facts, not wishes)

- A **session** is one conversation with its own context. You start it (`grok` in a repo), resume it, or start `/new`.
- The **dashboard** lists sessions. You dispatch new ones. That is how the *human* spawns DesignArchitect-warm, KanbanMaster-cold, and Worker-cold.
- **Subagents** are child sessions the *agent* can spawn. Each child has its own context. Isolation is `none` (shared working tree, default) or `worktree` (isolated git checkout under `~/.grok/worktrees/...`).
- **Worktrees** exist so parallel editors do not overwrite each other. They are a second working copy, not `dev`.
- **AGENTS.md** (and `.grok/rules/`, `.grok/agents/`) load every session in that tree. That is the natural home for init-file rules at Gate 4.
- **Skills / plugins / MCP** exist. They do not replace living-knowledge.
- **Plan mode** exists. It can host the approach-discussion step; it does not replace human approval.
- **Memory** (from 2026-09) writes its own markdown notes across sessions. That is a second memory store beside living-knowledge.

### 14.2 Decisions (locked to match the rest of this spec)

1. **Human spawns the three roles.** You start or resume a session from the dashboard (or a separate terminal) and point it at the matching init file / agent definition. Worker-cold must not spawn DesignArchitect-warm. Worker-cold must not spawn KanbanMaster-cold.
2. **One write-capable Worker-cold at a time** still holds. Grok Build can fan out many subagents and even workflow runs of hundreds of agents. This workflow forbids that for implementation. A Worker-cold session must not spawn write-capable subagents onto the app tree.
3. **Shared working tree on `dev`.** Ticket files, `kanban.md`, living-knowledge, and app code must be one tree the human pulls and tests. Do **not** put Worker-cold, KanbanMaster-cold, or DesignArchitect-warm in isolated worktrees for normal work. A worktree would fork the board.
4. **DesignArchitect-warm = resume the same session** when you can. That is what “warm” means on this platform. After a reset, it re-reads the design spec + living-knowledge headers + open TA tickets.
5. **KanbanMaster-cold and Worker-cold = new session per ceremony / per assignment** (or `/new`). Do not resume a month-old worker thread as if it were still cheap.
6. **Living-knowledge + tickets + this spec are source of truth.** Grok Build Memory is a cache. Durable facts must be written to git-backed living-knowledge or a ticket. Do not treat `/memory` notes as binding. If Memory contradicts a living-knowledge file, the file wins.
7. **Init files at Gate 4** will be Grok Build agent definitions (likely `.grok/agents/` plus pointers from `AGENTS.md`). The names stay `DesignArchitect-warm`, `KanbanMaster-cold`, `Worker-cold`. Exact file format is Gate 4, not a new role design.
8. **Plan mode** may be used for Step 4 (approach discussion). Implementation still waits for the human.
9. **Separate terminals are fine.** One terminal window per role, each `cd`’d into the same project folder and running `grok`, is a valid way to spawn the three roles. Each terminal must start a **new session** (not resume the same session). The working tree is shared because no worktree is used.

### 14.3 Still not a vendor fact

- Weekly token quota. Keep the user planning assumption (~6M) until replaced (§5.8).
- Exact on-disk schema of `.grok/agents/` on the day we implement.
- Whether project Memory can be turned off. If it cannot, rule 6 still applies: files win.

### 14.4 Why worktrees are rejected for this team

Worktrees solve *parallel writers*. This team is specified as one implementation writer plus markdown-only overview roles on the same board. Isolated checkouts would mean two `kanban.md` files and two ticket folders. That is the two-boards defect again. If the user later wants two workers, reopen worktrees then — not now.

### 14.5 Guards against the shared-tree failures

These five rules exist so the four concrete failures (stale ticket folders, stale `kanban.md`, unseen living-knowledge edits, changes invisible on `dev`) cannot happen silently.

1. **Session-start check.** Before any write, every role confirms it is **not** inside a worktree and that its working directory is the shared checkout. A cheap test: `git rev-parse --show-toplevel` and check that the path does not contain `.grok/worktrees`. If it is under a worktree path, stop immediately, tell the human, and do not edit tickets, living-knowledge, or the design spec. Do not attempt an apply.
2. **Commit + push after board or living-knowledge changes.** Ticket moves, `kanban.md` regeneration, and living-knowledge edits are not done until they are committed and pushed to `dev`. The human’s test is “pull `dev`”; anything that exists only in a local working tree is still invisible.
3. **Human-visible proof.** When a worker moves a ticket or a kanban master regenerates the board, the agent must say in the same message: the absolute path it wrote, the commit hash (or “pushed to origin/dev”), and a one-line “you should see this after `git pull`”. If it cannot name a commit, it has not finished.
4. **Session naming.** Rename each session on start (`/rename DesignArchitect-warm`, `/rename KanbanMaster-cold`, `/rename Worker-cold`). This makes the three terminals distinguishable and prevents accidental resume of the wrong conversation.
5. **No subagent isolation.** Init files forbid `isolation: worktree`. A worker that spawns a child must either not spawn a writer, or the child must also run on the shared tree (`isolation: none`).

These rules go into the three init files at Gate 4. They are also the content that the human-guide files (§14.6) must explain in plain language.

### 14.6 Human-guide files (how-to-for-human)

Once a project is started (Phase 1 begin), three markdown files are generated for the human only. They are not living-knowledge for agents. They are a short operating manual for the person who spawns and talks to the agents.

Proposed path:

```text
/app-name-1/human-guides/DesignArchitect-warm.md
/app-name-1/human-guides/KanbanMaster-cold.md
/app-name-1/human-guides/Worker-cold.md
```

Each file must answer, in plain language:

- What this role is for (and what it is not for).
- Warm or cold: whether to resume the same session or start a new one each time.
- When to start it (Phase 1, ceremony, ticket assignment, blocker, etc.).
- How to start it (separate terminal, `cd` into the shared checkout, `grok`, new session, `/rename`).
- What it is allowed to write, and what it must never write.
- The session-start worktree check and the commit-and-push proof it must give you.
- How you interact with it (approach approval, test loop, escalation, Phase 1 exit question).
- One concrete example of a good first message and a bad first message.

These files are generated from this workflow spec (not invented per project). They may be regenerated if the workflow spec changes, but they are not the place for project-specific architecture. Agents must not treat them as living-knowledge.
