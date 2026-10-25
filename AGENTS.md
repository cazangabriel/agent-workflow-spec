### 19. No unresolved term, in any written form

This is not a rule about summaries. It applies to every sentence you write into git: a rule, a prohibition, a path, a command, a role name, a step.

Information is insufficient when a reader who has only that file cannot resolve a term to one specific thing. The reader must be able to say what the thing is, where it lives, and whether it is a platform command, a file in a named repo, or a function the product defines.

This does not require the four-part rule shape on every sentence. A definition in the same file is enough. The four-part shape remains required for a rule another agent must apply.

Required: before you write a term, ask whether a cold reader can name the thing. If not, define it in the same file.

Violation: a bullet or a command that a cold reader could map to more than one kind of thing.

Example: "do not call `/remember`." A cold reader cannot tell whether that is a Grok Build slash command, a framework function, or a product feature. The sufficient form: "`/remember` is the Grok Build slash command that writes a note into Grok Build Memory, the optional cache this workflow disables. It is not a function in this repo. Do not call it."

## Role and tree

- Your role is whichever init file under `.grok/agents/` the human tells you to read. `/rename` is the Grok Build command that sets the session title. It does not set your role. Do not infer a role from a session title or from being in this folder.
- If the human has not named an init file, ask which file under `.grok/agents/` to read, then wait.
- A worktree is an isolated second checkout Grok Build can create. Its path contains `.grok/worktrees`. This workflow does not use them. If your path contains `.grok/worktrees`, stop and tell the human. Do not write.
- Grok Build Memory is an optional automatic note cache in the Grok Build product. This workflow disables it. `/remember` is the Grok Build slash command that writes into that cache. It is not a function in this repo. Do not call it. Do not treat Memory notes as binding.
- Section files under `initial_spec/sections/` in this repository are the source of truth for the workflow. Chat memory is not.
- A product repo is a separate repository where the three product roles run. Do not edit a product repo from this repository.
- The files `.grok/agents/design-architect-warm.md`, `kanban-master-cold.md`, and `worker-cold.md` are templates to copy into a product repo. Do not act as those roles here unless the human explicitly says this session is a dry run. A dry run means the human has said this session is practicing the template, not writing a product.
- `for-human-only/` is a folder in this repo that holds prompts for the human to paste into other conversations. Do not read it. The precision rules you obey are the ones in this file and in the init file the human told you to read.
