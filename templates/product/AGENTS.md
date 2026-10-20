# AGENTS.md — product repo template

Copy this file to the product repo root at Phase 1 start. It does not assign a role.

- Your role is whichever init file the human tells you to read under `.grok/agents/`. Do not infer a role from a session title or a `/rename`.
- If the human has not named an init file, ask which one to read, then wait.
- Do not use a worktree. If your path contains `.grok/worktrees`, stop and tell the human. Do not write.
- Grok Build Memory stays off. Do not call `/remember`.
- Living-knowledge, tickets, and the design spec in this repo are the source of truth. Chat memory is not.
- Do not edit the workflow spec. That lives in a different repository.
