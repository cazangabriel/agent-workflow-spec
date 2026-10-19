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
- **Memory** (from 2026-09) is an optional automatic markdown cache of conventions and decisions. It is off by default. It is a generic container; it does not implement the living-knowledge rules in this spec.

### 14.2 Decisions (locked to match the rest of this spec)

1. **Human spawns the three roles.** You start or resume a session from the dashboard (or a separate terminal) and point it at the matching init file / agent definition. Worker-cold must not spawn DesignArchitect-warm. Worker-cold must not spawn KanbanMaster-cold.
2. **One write-capable Worker-cold at a time** still holds. Grok Build can fan out many subagents and even workflow runs of hundreds of agents. This workflow forbids that for implementation. A Worker-cold session must not spawn write-capable subagents onto the app tree.
3. **Shared working tree on `dev`.** Ticket files, `kanban.md`, living-knowledge, and app code must be one tree the human pulls and tests. Do **not** put Worker-cold, KanbanMaster-cold, or DesignArchitect-warm in isolated worktrees for normal work. A worktree would fork the board.
4. **DesignArchitect-warm = resume the same session** when you can. That is what “warm” means on this platform. After a reset, it re-reads the design spec + living-knowledge headers + open TA tickets.
5. **KanbanMaster-cold and Worker-cold = new session per ceremony / per assignment** (or `/new`). Do not resume a month-old worker thread as if it were still cheap.
6. **Grok Build Memory is disabled for this workflow.** Living-knowledge and tickets are the designed contents; Memory is a simpler automatic container that would interfere. Do not enable it. Force it off if a config or managed setting has turned it on.
7. **Init files at Gate 4** will be Grok Build agent definitions (likely `.grok/agents/` plus pointers from `AGENTS.md`). The names stay `DesignArchitect-warm`, `KanbanMaster-cold`, `Worker-cold`. Exact file format is Gate 4, not a new role design.
8. **Plan mode** may be used for Step 4 (approach discussion). Implementation still waits for the human.
9. **Separate terminals are fine.** One terminal window per role, each `cd`’d into the same project folder and running `grok`, is a valid way to spawn the three roles. Each terminal must start a **new session** (not resume the same session). The working tree is shared because no worktree is used.

### 14.3 How to keep Memory off

Memory is off by default. Project `.grok/config.toml` does **not** control it (that file only contributes MCP, plugins, and permissions). Disable is user-level or per process.

Preferred for every session in this workflow:

```bash
grok --no-memory
```

or

```bash
export GROK_MEMORY=0
grok
```

Persistent user default (if Memory was ever turned on):

```toml
# ~/.grok/config.toml
[memory]
enabled = false
```

`--no-memory` and `GROK_MEMORY=0` force-disable even if TOML says otherwise. Confirm with `grok inspect` if unsure. Agents must not call `/remember` or rely on `/memory` notes.

### 14.4 Still not a vendor fact

- Weekly token quota. xAI does not publish a precise weekly number. The ~6M figure is a user planning heuristic only (§5.8). The human may form an informal estimate over time from observed burn. The escalate band remains a rough “ask first” signal the human can adjust.
- Exact on-disk schema of `.grok/agents/` on the day we implement.

### 14.5 Why worktrees are rejected for this team

Worktrees solve *parallel writers*. This team is specified as one implementation writer plus markdown-only overview roles on the same board. Isolated checkouts would mean two `kanban.md` files and two ticket folders. That is the two-boards defect again. If the user later wants two workers, reopen worktrees then — not now.

### 14.6 Guards against the shared-tree failures

These five rules exist so the four concrete failures (stale ticket folders, stale `kanban.md`, unseen living-knowledge edits, changes invisible on `dev`) cannot happen silently.

1. **Session-start check.** Before any write, every role confirms it is **not** inside a worktree and that its working directory is the shared checkout. A cheap test: `git rev-parse --show-toplevel` and check that the path does not contain `.grok/worktrees`. If it is under a worktree path, stop immediately, tell the human, and do not edit tickets, living-knowledge, or the design spec. Do not attempt an apply.
2. **Commit + push after board or living-knowledge changes.** Ticket moves, `kanban.md` regeneration, and living-knowledge edits are not done until they are committed and pushed to `dev`. The human’s test is “pull `dev`”; anything that exists only in a local working tree is still invisible.
3. **Human-visible proof.** When a worker moves a ticket or a kanban master regenerates the board, the agent must say in the same message: the absolute path it wrote, the commit hash (or “pushed to origin/dev”), and a one-line “you should see this after `git pull`”. If it cannot name a commit, it has not finished.
4. **Session naming.** Rename each session on start (`/rename DesignArchitect-warm`, `/rename KanbanMaster-cold`, `/rename Worker-cold`). This makes the three terminals distinguishable and prevents accidental resume of the wrong conversation.
5. **No subagent isolation.** Init files forbid `isolation: worktree`. A worker that spawns a child must either not spawn a writer, or the child must also run on the shared tree (`isolation: none`).

These rules go into the three init files at Gate 4. They are also the content that the human-guide files (§14.7) must explain in plain language.

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
