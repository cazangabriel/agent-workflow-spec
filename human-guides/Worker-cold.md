# How to work with Worker-cold

This file is for you. Agents do not treat it as living-knowledge.

## What this role is for

Worker-cold implements one ticket after you approve the approach. It shows you `kanban.md` first, you pick the ticket, it proposes an approach, you agree, then it implements, commits to `dev`, and moves the ticket to `in-testing`.

You test (or gut-skip). You move the ticket to `done` on pass, or back to `in-progress` on fail.

## What it is not for

- It does not write living-knowledge or the design spec.
- It does not create tickets.
- It does not mark a ticket `done` or `closed`.
- It does not edit the workflow spec.
- It must not spawn DesignArchitect-warm or KanbanMaster-cold.

## Warm or cold

Cold by default: new session per assignment.

Reuse the **same** session only when you next pick a ticket in the same module, or one that Depends on the ticket just finished. Start a new session when the next ticket is a different module with no dependency.

## When to start it

- You have an `open` ticket you want implemented.
- You are continuing a related ticket on the same worker instance.

You drive the board. Several tickets may be `in-progress` or `in-testing` at once. There is no rule that the worker must wait for testing to finish before you start another ticket.

## How to start it

1. Open a terminal of its own.
2. `cd` into the **product** repo, shared checkout, no worktree.
3. Start with Memory off:

```bash
grok --no-memory
```

4. Choose **new session**, unless you are deliberately reusing this instance on a related ticket.
5. Rename it:

```text
/rename Worker-cold
```

6. Point it at its init file (`Worker-cold.md`).
7. First message:

> Confirm you are not in a worktree. Show me `kanban.md`. Then suggest which open ticket to take next and wait for me to pick.

## How you interact

1. It shows the board.
2. You pick a ticket.
3. It proposes an approach. You approve or change it. Implementation starts only after that.
4. It commits and pushes to `dev`, moves the ticket to `in-testing`, and tells you the path and the commit.
5. You pull `dev` and test, or gut-skip.
6. Pass: you move the file `in-testing` → `done`. Fail: you move it back; the worker increments friction and continues.

If it hits the stop-rule (unsure, missing living-knowledge, looks like a design change), it comments, tells you, and does not invent the rule. You decide whether to spawn KanbanMaster-cold.

A worker ticket that looks enormous is a split request, not a reason to spend a million tokens.

## What it may write

- Application code for the agreed approach
- Comments on the ticket it is executing
- Ticket moves among `open` / `in-progress` / `in-testing` / `blocked` for that ticket
- Commits on `dev`

## What it must never write

- New tickets
- Living-knowledge or the design spec
- `done` or `closed`
- The workflow spec

## Good first message

> Confirm you are not in a worktree. Show me kanban.md and suggest the next open ticket. Wait for me to pick before proposing an approach.

## Bad first message

> Implement whatever is on the board and mark it done.

That skips the pick, the approach approval, and the human test.
