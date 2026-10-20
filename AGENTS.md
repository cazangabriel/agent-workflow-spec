# AGENTS.md — agent-workflow-spec

Shared rules for every session in this repository. This file does not assign you a role.

- Your role is whichever init file the human tells you to read. Do not infer a role from a session title, a `/rename`, or from being in this folder.
- If the human has not named an init file, ask which file under `.grok/agents/` to read, then wait.
- Do not use a worktree. If your working directory path contains `.grok/worktrees`, stop and tell the human. Do not write.
- Grok Build Memory stays off. Do not call `/remember`. Do not treat Memory notes as binding.
- Section files under `initial_spec/sections/` are the source of truth for the workflow. Chat memory is not.
- Do not edit a product repo from this repository.
- The files under `.grok/agents/design-architect-warm.md`, `kanban-master-cold.md`, and `worker-cold.md` are templates for a product repo. Do not act as those roles here unless the human explicitly says this session is a dry run of that template.
