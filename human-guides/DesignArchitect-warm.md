# How to work with DesignArchitect-warm

This file is for you. Agents do not treat it as living-knowledge.

## What this role is for

DesignArchitect-warm owns the product design. In Phase 1 it works with you until the design spec is good enough to cut tickets from: bounded contexts, ubiquitous language, 3–4 architectural characteristics, module and layer map, technology choices, component order, and the two abbreviation lists.

It also handles TA tickets in Phase 2: design problems that KanbanMaster-cold wrote. It writes the solution into the design spec or living-knowledge, not into a worker ticket.

## What it is not for

- It does not cut ordinary feature tickets.
- It does not read the kanban board or non-TA tickets unless a TA ticket is incomplete.
- It does not edit the workflow spec. That is WorkflowSteward-warm, in the workflow-spec repo.

## Warm or cold

Warm. Resume the same session when you can, so design discussion stays in one place.

After a reset it must re-read the design spec, living-knowledge file headers, and any open TA tickets before advising.

## When to start it

- Start of Phase 1 (first time you design the product).
- A TA ticket is ready and you want a design answer.
- You declare Mini Phase 1 (a new product area the current spec does not cover).

## How to start it

1. Open a terminal of its own. Do not share this terminal with another role.
2. `cd` into the **product** repo, on the shared checkout. Do not use a worktree.
3. Start with Memory off:

```bash
grok --no-memory
```

4. Resume the existing session if you already have one for this role. Otherwise choose **new session**.
5. First message (this assigns the role; `/rename` is not required):

> Read `.grok/agents/design-architect-warm.md` and obey it. Confirm you are not in a worktree. Then read the design spec and the living-knowledge file headers, and wait.

## How you interact

- In Phase 1 you discuss structure with it until you can answer: “Do you accept Phase 1 as done for this spec version?”
- On a TA ticket, it writes the solution into the design spec and leaves a concise comment on the TA ticket.
- If a design change looks like it would burn a large share of the week’s tokens, it must stop and ask you first (escalate band).

## What it may write

- The design spec
- Living-knowledge when a design change requires it
- The abbreviation lists in Phase 1
- Comments on TA tickets

## What it must never write

- Ordinary F tickets
- The workflow spec
- `kanban.md` by hand

## Good first message

> Read `.grok/agents/design-architect-warm.md` and obey it. Confirm you are not in a worktree. Then summarise what Phase 1 still needs before I can accept it.

## Bad first message

> Look at the kanban board and start implementing tickets.

That is Worker-cold’s job, and it is forbidden from reading the board.
