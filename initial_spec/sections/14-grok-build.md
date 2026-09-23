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

1. **Human spawns the three roles.** You start or resume a session from the dashboard and point it at the matching init file / agent definition. Worker-cold must not spawn DesignArchitect-warm. Worker-cold must not spawn KanbanMaster-cold.
2. **One write-capable Worker-cold at a time** still holds. Grok Build can fan out many subagents and even workflow runs of hundreds of agents. This workflow forbids that for implementation. A Worker-cold session must not spawn write-capable subagents onto the app tree.
3. **Shared working tree on `dev`.** Ticket files, `kanban.md`, living-knowledge, and app code must be one tree the human pulls and tests. Do **not** put Worker-cold, KanbanMaster-cold, or DesignArchitect-warm in isolated worktrees for normal work. A worktree would fork the board.
4. **DesignArchitect-warm = resume the same session** when you can. That is what “warm” means on this platform. After a reset, it re-reads the design spec + living-knowledge headers + open TA tickets.
5. **KanbanMaster-cold and Worker-cold = new session per ceremony / per assignment** (or `/new`). Do not resume a month-old worker thread as if it were still cheap.
6. **Living-knowledge + tickets + this spec are source of truth.** Grok Build Memory is a cache. Durable facts must be written to git-backed living-knowledge or a ticket. Do not treat `/memory` notes as binding. If Memory contradicts a living-knowledge file, the file wins.
7. **Init files at Gate 4** will be Grok Build agent definitions (likely `.grok/agents/` plus pointers from `AGENTS.md`). The names stay `DesignArchitect-warm`, `KanbanMaster-cold`, `Worker-cold`. Exact file format is Gate 4, not a new role design.
8. **Plan mode** may be used for Step 4 (approach discussion). Implementation still waits for the human.

### 14.3 Still not a vendor fact

- Weekly token quota. Keep the user planning assumption (~6M) until replaced (§5.8).
- Exact on-disk schema of `.grok/agents/` on the day we implement.
- Whether project Memory can be turned off. If it cannot, rule 6 still applies: files win.

### 14.4 Why worktrees are rejected for this team

Worktrees solve *parallel writers*. This team is specified as one implementation writer plus markdown-only overview roles on the same board. Isolated checkouts would mean two `kanban.md` files and two ticket folders. That is the two-boards defect again. If the user later wants two workers, reopen worktrees then — not now.
