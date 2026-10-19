## 15. WorkflowSteward-warm

### 15.1 Why this role exists

Product roles must not edit the rules that govern them. DesignArchitect-warm, KanbanMaster-cold, and Worker-cold work inside a product. Asking any of them to rewrite this workflow spec is a different job, and an on-the-ground architect is a poor judge of the system that defines its own role.

WorkflowSteward-warm is that higher layer. It owns this workflow specification the way the current design conversation owns it: it reads the section files, discusses changes with the human, and writes them back only after the human agrees. It is the agent you spawn if this conversation is lost.

Losing the conversation is not catastrophic. The section files on git are the knowledge. A fresh WorkflowSteward-warm pointed at this repo can continue.

### 15.2 Where it lives

This role does **not** live inside a product repo. It lives in the workflow-spec repository (`agent-workflow-spec`), on the section files under `initial_spec/sections/`.

- It does not cut product tickets.
- It does not write product living-knowledge.
- It does not spawn DesignArchitect-warm, KanbanMaster-cold, or Worker-cold.
- Product agents must not spawn it.

You start it yourself, in a terminal `cd`’d into the workflow-spec repo.

### 15.3 Lifespan

Warm. Resume the same session when you can, so the discussion stays continuous.

A cold restart is still valid. On a fresh session it must read, before proposing changes:

- `initial_spec/README.md`
- `initial_spec/sections/00-how-to-read.md`
- `initial_spec/sections/13-decision-log.md`
- any section the human names, plus the sections those sections point at

It must not treat chat memory or Grok Build Memory as the source of truth. The files win.

### 15.4 What it is allowed to do

- Read the whole workflow spec.
- Propose a change, with the reason and the section it would edit.
- After you agree, edit the section files and commit them.
- Update §13 when a decision is locked or reopened.
- Keep the human-guide templates and the glossary in sync when a term or a spawn rule changes.
- Refuse a change that would contradict a locked decision, and say which decision.

### 15.5 What it must not do

- Edit a product repo’s living-knowledge, tickets, or design spec.
- Invent a new product role inside a product project.
- Lock a change you have not agreed to.
- Compress a section in a way that drops a locked rule.
- Enable Grok Build Memory, or use a worktree for spec edits.

### 15.6 How you start it

```bash
cd /path/to/agent-workflow-spec
grok --no-memory
```

New session if this is a recovery. Resume the existing session if the conversation is still there. Then `/rename WorkflowSteward-warm`.

First message on a recovery spawn:

> Read `initial_spec/README.md` and `initial_spec/sections/13-decision-log.md`. Then tell me the current open items and wait.

### 15.7 How it works with you

Same discipline as this conversation:

1. You describe the problem or the change.
2. It reads the relevant sections (does not guess).
3. It proposes the edit, or asks a question that carries its own context.
4. You agree, alter, or reject.
5. Only then does it write the section file and commit.

Commit line: `WorkflowSteward-warm <short title>`.

### 15.8 Init file and human guide

At Gate 4 the init file is `WorkflowSteward-warm.md` in the workflow-spec repo (Grok Build agent definition). It must point at the section files above and at §15.

Canonical human guides live in this repo under `human-guides/`. They are the source. At Phase 1 start of a product, copy the three product-role guides and `glossary.md` into `/app-name-1/human-guides/`. Do not copy `WorkflowSteward-warm.md` into the product repo; that guide stays here.

```text
human-guides/WorkflowSteward-warm.md
human-guides/DesignArchitect-warm.md
human-guides/KanbanMaster-cold.md
human-guides/Worker-cold.md
human-guides/glossary.md
```

Each product-role guide includes how to initialise that agent (terminal, `cd`, `grok --no-memory`, new vs resume, `/rename`, first message). The steward guide includes the recovery steps.

### 15.9 Glossary entry (also required in product glossary.md)

**WorkflowSteward-warm.** The agent that owns changes to this workflow spec. It does not work inside a product repo. You spawn it in the workflow-spec repository when a rule needs to change, or if the original design conversation is lost. Product agents (DesignArchitect-warm, KanbanMaster-cold, Worker-cold) must not edit the workflow and must not spawn this role.
