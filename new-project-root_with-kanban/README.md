# new-project-root_with-kanban

Copy the contents of this folder into a new product repository.

Rename `app-name-1` to the product name.
Do not copy any steward AGENTS.md or the agent-workflow-spec itself.

Product agents read only their own init file under `.grok/agents/` and the shared files listed in that init file.
WorkflowSteward-warm is started separately if the workflow specification needs to change.
