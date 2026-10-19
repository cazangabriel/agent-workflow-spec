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
