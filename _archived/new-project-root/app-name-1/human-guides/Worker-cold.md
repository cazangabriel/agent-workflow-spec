# How to work with Worker-cold

This file is for you. Agents do not treat it as living-knowledge.

## What this role is for

Worker-cold implements one ticket after you approve the approach. It shows you `kanban.md` first, you pick the ticket, it proposes an approach, you agree, then it implements, commits to `dev`, and moves the ticket to `in-testing`.

You test (or gut-skip). You signal that the ticket is done. The worker then moves it to `done`. You do not have to move the file.

## What it is not for

- It does not write living-knowledge or the design spec.
- It does not create tickets.
- It does not mark a ticket `done` before you signal, and it does not mark `closed`.
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
5. First message (this assigns the role; `/rename` is not required):

> Read `.grok/agents/worker-cold.md` and obey it. Confirm you are not in a worktree. Show me `kanban.md`. Suggest which open ticket to take next and wait for me to pick.

## How you interact

1. It shows the board.
2. You pick a ticket.
3. It proposes an approach. You approve or change it. Implementation starts only after that.
4. It commits and pushes to `dev`, moves the ticket to `in-testing`, and tells you the path and the commit.
5. You pull `dev` and test, or gut-skip.
6. Pass: you say the ticket is done. The worker moves the file `in-testing` → `done`. Fail: you say it failed; the worker increments friction and continues.

The done signal is you saying the test passed, gut-skipping, or saying to consider the ticket done. Silence is not a signal. The worker does not move to `done` before you say so. The worker does not move to `closed`. KanbanMaster-cold does that.

If it hits the stop-rule (unsure, missing living-knowledge, looks like a design change), it comments, tells you, and does not invent the rule. You decide whether to spawn KanbanMaster-cold.

A worker ticket that looks enormous is a split request, not a reason to spend a million tokens.

## What it may write

- Application code for the agreed approach
- Comments on the ticket it is executing
- Ticket moves among `open` / `in-progress` / `in-testing` / `blocked` for that ticket
- A move to `done`, only after you signal
- Commits on `dev`

## What it must never write

- New tickets
- Living-knowledge or the design spec
- `done` before you signal, or `closed`
- The workflow spec

## Good first message

> Read `.grok/agents/worker-cold.md` and obey it. Confirm you are not in a worktree. Show me kanban.md and suggest the next open ticket. Wait for me to pick before proposing an approach.

## Bad first message

> Implement whatever is on the board and mark it done.

That skips the init file, the pick, the approach approval, and the human test.
