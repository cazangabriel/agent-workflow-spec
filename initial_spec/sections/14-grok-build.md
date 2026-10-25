## 14. Grok Build platform constraints

This section maps the workflow onto Grok Build. Platform details can change. If a later release contradicts a rule here, raise it. Do not silently follow the platform against this spec.
Violation: an agent enabling a platform feature this section forbids, because the platform default allows it.
Example: Grok Build can create a worktree. This section forbids it. The agent stops. It does not use the worktree because the platform offered it.

### 14.2 Decisions (locked)

1. Human spawns the three roles. You start or resume a session and point it at the matching init file. Worker-cold must not spawn DesignArchitect-warm or KanbanMaster-cold.
   Violation: the worker spawning KanbanMaster-cold itself.
   Example: the worker is blocked. It comments and tells the human. It does not spawn the ceremony.

2. One write-capable Worker-cold at a time. A Worker-cold session must not spawn write-capable subagents onto the app tree.
   Violation: a worker spawning a child that writes files.
   Example: the worker does not spawn a second agent to "write the migration." It writes the migration itself, or it stops.

3. Shared working tree on `dev`. Do not put any role in an isolated worktree for normal work. A worktree would fork the board.
   Violation: a session whose path contains `.grok/worktrees` continuing to write.
   Example: path is `/repo/.grok/worktrees/abc`. The agent stops and tells the human. It does not edit `kanban.md`.

4. DesignArchitect-warm = resume the same session when you can. After a reset it re-reads the design spec, living-knowledge headers, and open TA tickets.
   Violation: starting a fresh DesignArchitect-warm and treating chat memory as the spec.

5. KanbanMaster-cold and Worker-cold = new session per ceremony or assignment. Do not resume a month-old worker thread as if it were still cheap.
   Violation: resuming a month-old worker thread for a new ticket in a different module.

6. Grok Build Memory is disabled. Do not enable it. Do not call `/remember`.
   Violation: an agent calling `/remember` to store a convention.
   Example: the convention goes in living-knowledge. Not in Memory.

7. Init files are `.grok/agents/`. Role identity is the file the human tells the session to read. `/rename` is not required.
   Violation: inferring the role from a session title.
   Example: the human says "read `.grok/agents/worker-cold.md`." That is the role. A session titled "architect" does not override it.

8. Plan mode may be used for the approach-discussion step. Implementation still waits for the human.
   Violation: leaving plan mode and writing code before the human agrees.

9. Separate terminals are fine. One terminal per role, each `cd`'d into the same folder, each a new session unless resuming DesignArchitect-warm or reusing a Worker-cold on a related ticket.
   Violation: two terminals resuming the same worker session.

### 14.5 Why worktrees are rejected

Worktrees solve parallel writers. This team is one implementation writer plus markdown-only roles on the same board. Isolated checkouts would mean two `kanban.md` files and two ticket folders. That is the two-boards defect. If the user later wants two workers, reopen worktrees then.
Violation: using a worktree "so the worker cannot collide with itself."
Example: one worker, shared tree. No worktree.

### 14.6 Guards

1. Session-start check. Before any write, `git rev-parse --show-toplevel`. If the path contains `.grok/worktrees`, stop. Do not write. Do not attempt an apply.
   Violation: writing a ticket from a worktree path.
   Example: path contains `.grok/worktrees`. The agent says so and stops. It does not move a ticket file.

2. Commit and push after board or living-knowledge changes. The human's test is "pull `dev`". Anything only in a local tree is invisible.
   Violation: "done" with no push.

3. Human-visible proof. When a ticket moves or the board regenerates, the same message names the absolute path, the commit, and that the human should see it after `git pull`. If it cannot name a commit, it has not finished.
   Violation: "pushed" with no path and no commit.
   Example: "Moved `/app-name-1/tickets/in-testing/AM-10-F-DB-add-recipe-table.md`. Commit `abc123` on `dev`. You should see it after `git pull`."

4. Role comes from the init file. Do not infer a role from a session title.

5. No subagent isolation. A worker that spawns a child must not spawn a writer, or the child must run on the shared tree.
   Violation: a child running in a worktree.

### 14.3 How to keep Memory off

Preferred: `grok --no-memory` or `export GROK_MEMORY=0`. Persistent: `~/.grok/config.toml` with `[memory] enabled = false`. `--no-memory` and `GROK_MEMORY=0` force-disable even if TOML says otherwise. Agents must not call `/remember`.
Violation: starting `grok` with no flag when Memory has been turned on in user config, and not checking.

### 14.4 Still not a vendor fact

Weekly token quota is not published. The ~6M figure is a user planning heuristic (§5.8). Activation is: start `grok --no-memory`, then tell it to read the init file.

### 14.7 and 14.8

Human-guide paths and glossary entries are unchanged. Agents must not treat human-guides as living-knowledge. Init files do not depend on them. The glossary definitions in §14.8 remain the plain-language meanings for the human.
