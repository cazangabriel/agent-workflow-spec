# How to work with WorkflowSteward-warm

This file is for you. Agents do not treat it as living-knowledge.

## What this role is for

WorkflowSteward-warm owns changes to the workflow spec itself. It does the job this design conversation has been doing: read the section files, discuss a change with you, write it only after you agree.

It does **not** work inside a product repo. It does not cut product tickets, write product living-knowledge, or spawn DesignArchitect-warm, KanbanMaster-cold, or Worker-cold.

Product agents must not edit the workflow spec and must not spawn this role.

## Warm or cold

Warm. Resume the same session when the conversation is still there.

A new session is fine, and is the recovery path if this conversation is lost. The knowledge is the section files on git, not the chat.

## When to start it

- A workflow rule needs to change.
- Something in the product team is painful and you suspect the workflow, not the product design.
- You lost the original design conversation and need to continue.

## How to start it (new session / recovery)

1. Open a terminal.
2. `cd` into the workflow-spec repo (`agent-workflow-spec`), not into a product repo.
3. Start Grok Build with Memory off:

```bash
grok --no-memory
```

4. When it offers previous sessions, choose **new session**. Do not resume a product-agent session.
5. First message (this is what gives it the role; `/rename` is not required):

> Read `.grok/agents/workflow-steward-warm.md` and obey it. Then read `initial_spec/README.md` and `initial_spec/sections/13-decision-log.md`. Tell me the current open items and wait.

6. Wait for it to report the open items. Then tell it what you want to change.

It must not write a section file until you agree to the proposed edit.

## How to resume an existing session

Same `cd` and `grok --no-memory`. Resume the session you already used for this role. You do not need `/rename` to keep the role; the init file is what assigned it.

## What it may write

- Section files under `initial_spec/sections/`
- `initial_spec/README.md`
- These human-guide files, when a spawn rule or a glossary term changes
- `.grok/agents/` in this repo

## What it must never write

- Anything in a product repo
- Product tickets, product living-knowledge, or a product design spec

## Checks you should see

- It starts with `--no-memory` (or `GROK_MEMORY=0`).
- It is not inside a worktree. If it is, it must stop and tell you.
- After an agreed edit it commits and tells you the path and the commit, so you can pull.

## Good first message

> Read `.grok/agents/workflow-steward-warm.md` and obey it. Then read `initial_spec/README.md` and `initial_spec/sections/13-decision-log.md`. Tell me the current open items and wait.

## Bad first message

> Just fix the workflow however you think is best.

That skips the init file, the read, and the agreement step.
