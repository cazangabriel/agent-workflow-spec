# Product root template

Copy the contents of this folder into a new product repository root. Then rename `app-name-1` to the real app name.

This folder is the only thing you copy into a product repo.

Do not copy, from the parent `agent-workflow-spec` repo:

- the root `AGENTS.md` — that file is for WorkflowSteward-warm
- `.grok/agents/workflow-steward-warm.md`
- `initial_spec/`
- `for-human-only/`

Work on a `dev` branch. You release to `main`. Start every product agent with `grok --no-memory`.
