## 14. Grok Build platform constraints

This section maps the workflow onto Grok Build. Platform details can change. If a later release contradicts a rule here, raise it. Do not silently follow the platform against this spec.
Violation: an agent enabling a platform feature this section forbids, because the platform default allows it.
Example: Grok Build can create a worktree. This section forbids it. The agent stops.

### 14.1 What Grok Build gives us (facts, not wishes)

- A session is one conversation with its own context. You start it (`grok` in a repo), resume it, or start `/new`.
- The dashboard lists sessions. You dispatch new ones. That is how the human spawns the roles.
- Subagents are child sessions the agent can spawn. Isolation is `none` (shared working tree, default) or `worktree` (isolated checkout under `~/.grok/worktrees/...`).
- Worktrees exist so parallel editors do not overwrite each other. They are a second working copy, not `dev`.
- `AGENTS.md` and `.grok/agents/` load every session in that tree.
- Skills, plugins, and MCP exist. They do not replace living-knowledge.
- Plan mode exists. It can host the approach-discussion step. It does not replace human approval.
- Memory is an optional automatic markdown cache. Off by default. A generic container. It does not implement the living-knowledge rules in this spec.

### 14.2 Decisions (locked)

1. Human spawns the three roles. Point the session at the matching init file. Worker-cold must not spawn DesignArchitect-warm or KanbanMaster-cold.
   Violation: the worker spawning KanbanMaster-cold itself.
   Example: the worker is blocked. It comments and tells the human. It does not spawn the ceremony.

2. One write-capable Worker-cold at a time. A Worker-cold session must not spawn write-capable subagents onto the app tree.
   Violation: a worker spawning a child that writes files.
   Example: the worker does not spawn a second agent to write the migration. It writes it, or it stops.

3. Shared working tree on `dev`. Do not put any role in an isolated worktree for normal work.
   Violation: a session whose path contains `.grok/worktrees` continuing to write.
   Example: path is `/repo/.grok/worktrees/abc`. The agent stops. It does not edit `kanban.md`.

4. DesignArchitect-warm = resume the same session when you can. After a reset it re-reads the design spec, living-knowledge headers, and open TA tickets.
   Violation: a fresh DesignArchitect-warm treating chat memory as the spec.

5. KanbanMaster-cold and Worker-cold = new session per ceremony or assignment. Do not resume a month-old worker thread as if it were still cheap.
   Violation: resuming a month-old worker thread for a ticket in a different module.

6. Grok Build Memory is disabled. Do not enable it. Do not call `/remember`.
   Violation: an agent calling `/remember` to store a convention.
   Example: the convention goes in living-knowledge. Not in Memory.

7. Init files are `.grok/agents/`. Role identity is the file the human tells the session to read. `/rename` is not required.
   Violation: inferring the role from a session title.
   Example: the human says "read `.grok/agents/worker-cold.md`." That is the role.

8. Plan mode may be used for the approach-discussion step. Implementation still waits for the human.
   Violation: leaving plan mode and writing code before the human agrees.

9. Separate terminals are fine. One terminal per role, each `cd`'d into the same folder, each a new session unless resuming DesignArchitect-warm or reusing a Worker-cold on a related ticket.
   Violation: two terminals resuming the same worker session.

### 14.3 How to keep Memory off

Preferred: `grok --no-memory` or `export GROK_MEMORY=0`. Persistent: `~/.grok/config.toml` with `[memory] enabled = false`. `--no-memory` and `GROK_MEMORY=0` force-disable even if TOML says otherwise. Agents must not call `/remember`.
Violation: starting `grok` with no flag when Memory has been turned on in user config, and not checking.

### 14.4 Still not a vendor fact

Weekly token quota is not published. The ~6M figure is a user planning heuristic (§5.8). Activation is: start `grok --no-memory`, then tell it to read the init file.

### 14.5 Why worktrees are rejected

Worktrees solve parallel writers. This team is one implementation writer plus markdown-only roles on the same board. Isolated checkouts would mean two `kanban.md` files and two ticket folders. If the user later wants two workers, reopen worktrees then.
Violation: using a worktree so the worker cannot collide with itself.
Example: one worker, shared tree. No worktree.

### 14.6 Guards

1. Session-start check. Before any write, `git rev-parse --show-toplevel`. If the path contains `.grok/worktrees`, stop. Do not write. Do not attempt an apply.
   Violation: writing a ticket from a worktree path.
   Example: path contains `.grok/worktrees`. The agent says so and stops.

2. Commit and push after board or living-knowledge changes. The human's test is pull `dev`.
   Violation: "done" with no push.

3. Human-visible proof. The same message names the absolute path, the commit, and that the human should see it after `git pull`.
   Violation: "pushed" with no path and no commit.
   Example: "Moved `/app-name-1/tickets/in-testing/AM-10-F-DB-add-recipe-table.md`. Commit `abc123` on `dev`. You should see it after `git pull`."

4. Role comes from the init file. Do not infer a role from a session title.

5. No subagent isolation. A worker that spawns a child must not spawn a writer, or the child must run on the shared tree.
   Violation: a child running in a worktree.

### 14.7 Human-guide files

Once a project is started, markdown files are generated for the human only. They are not living-knowledge for agents.

```text
/app-name-1/human-guides/DesignArchitect-warm.md
/app-name-1/human-guides/KanbanMaster-cold.md
/app-name-1/human-guides/Worker-cold.md
/app-name-1/human-guides/glossary.md
```

Each role file must answer, in plain language: what the role is for and not for; warm or cold; when to start it; how to start it; what it may write and must never write; the worktree check and the commit-and-push proof; how you interact with it; one good first message and one bad first message.

Agents must not treat them as living-knowledge. Init files do not depend on them.
Violation: an init file that says "read the human guide."
Example: the init file lists the spec sections it must obey. It does not point at `human-guides/Worker-cold.md`.

### 14.8 Glossary (human-readable)

Generated at `/app-name-1/human-guides/glossary.md`. Agents do not treat it as living-knowledge.

- Friction. A number on a ticket that starts at 0. Each fail (test fail, send-back, stop-rule) adds 1 and a comment. No upper limit. Not a difficulty rating.
- Living-knowledge. Files under `/app-name-1/living-knowledge/`. Workers do not write them. Source of truth, not Memory, not chat.
- Design spec. Phase 1 document. Only DesignArchitect-warm and the human write it.
- F ticket. Feature ticket for Worker-cold. One testable outcome plus one approach discussion.
- TA ticket. Design problem for DesignArchitect-warm. No solution. Only KanbanMaster-cold creates them.
- Warm. DesignArchitect-warm. Resume the same session when you can.
- Cold. KanbanMaster-cold and Worker-cold. New session each ceremony or assignment.
- Phase 1. Design work until the human accepts the design spec as done.
- Phase 2. Ticketed delivery against that accepted spec.
- Mini Phase 1. A new product area whose language the current spec does not cover. A feature inside an already-mapped module is not Mini Phase 1.
- Stop-rule. Worker stops when unsure, when living-knowledge does not cover it, or when it looks like a design change. Comments, tells the human, does not invent the rule.
- Escalate band. Ask-first signal for DesignArchitect-warm and KanbanMaster-cold. Worker tickets that large are split.
- Column folders. State is the folder. The filename does not contain the state.
- NEXT. `/app-name-1/tickets/NEXT`, one integer. Only KanbanMaster-cold updates it.
- Worktree. Isolated second checkout. This workflow does not use them.
- Grok Build Memory. Optional note-taking. Disabled for this workflow.
- Kanban ceremony. Fresh KanbanMaster-cold. Pulls tickets, reviews blockers, closes accepted done, regenerates `kanban.md`.
