# How to work with KanbanMaster-cold

This file is for you. Agents do not treat it as living-knowledge.

## What this role is for

KanbanMaster-cold runs the board. A ceremony is a fresh session that:

- reads the design spec
- cuts new backlog tickets from it
- moves backlog → open (target ~10, hard stop 15)
- reviews in-progress, in-testing, blocked, and done
- writes TA tickets when the gap is design-level (problem only, no solution)
- closes accepted done tickets
- regenerates `kanban.md`
- increments `/app-name-1/tickets/NEXT` when it cuts a ticket

## What it is not for

- It does not edit the design spec.
- It does not implement code.
- It does not invent module or layer abbreviations. Those come from living-knowledge.
- It does not edit the workflow spec.

## Warm or cold

Cold. New session per ceremony. Do not resume a month-old kanban thread.

## When to start it

- Backlog and open are empty or thin and you need more tickets.
- A worker is blocked and you agree the board role should look.
- Done tickets are waiting to be closed.
- You want a ceremony even if nothing is blocked.

Ceremonies cannot be skipped. Without one, no new backlog tickets can legally appear.

## How to start it

1. Open a terminal of its own.
2. `cd` into the **product** repo, shared checkout, no worktree.
3. Start with Memory off:

```bash
grok --no-memory
```

4. Choose **new session**. Do not resume the previous KanbanMaster-cold session.
5. Rename it:

```text
/rename KanbanMaster-cold
```

6. Point it at its init file (`KanbanMaster-cold.md`).
7. First message:

> Confirm you are not in a worktree. Read the design spec and `kanban.md`. Then run a ceremony: report what you would pull from backlog into open, and wait for me to agree before writing tickets.

## How you interact

- You spawn it. Workers do not.
- Extra work a worker discovers is a comment on the current ticket. You spawn KanbanMaster-cold to cut the follow-on ticket.
- If it cannot solve a blocker and the gap is design-level, it writes a TA ticket. You then spawn DesignArchitect-warm on that TA only.

## What it may write

- Non-TA tickets and TA tickets (problem only)
- Module living-knowledge (gotchas, conventions) — not the design spec
- `kanban.md` (generated)
- `/app-name-1/tickets/NEXT`

## What it must never write

- The design spec
- The workflow spec
- A TA body that is only “see F-33”
- Lazy slugs (`fix`, `update`, `misc`)

## Good first message

> Confirm you are not in a worktree. Read the design spec and kanban.md. Propose the next backlog → open pull and wait.

## Bad first message

> Continue from where the last KanbanMaster-cold left off and just do whatever seems right.

Cold means a new session that re-reads the files, not a resumed junk thread.
