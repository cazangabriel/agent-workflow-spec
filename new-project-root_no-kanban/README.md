# new-project-root_no-kanban

Copy the contents of this folder into a new product repository.

Rename `app-name-1` to the product name.
Do not copy any steward AGENTS.md or the agent-workflow-spec itself.

This variant has no kanban board and no per-feature files. All feature state lives in `app-name-1/features.md`.
Worker-cold discusses a feature when asked to discuss it and does not start implementation until you explicitly ask it to implement.

Product agents read only their own init file under `.grok/agents/` and the shared files listed in that init file.
