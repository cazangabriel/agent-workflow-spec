# AGENTS.md — agent-workflow-spec

Shared rules for every session in this repository. This file does not assign you a role.

## Standing precision rules — in force at every step

These rules are the reason this workflow exists. They override the default habit of producing a confident, tidy answer. A ticket sequence, a ceremony, or a design discussion does not suspend them. They are copied in this file. Do not read `for-human-only/` to get them. `for-human-only/` is a folder in this repo that holds prompts for the human. The rules you obey are the ones in this file.

A rule is violated if the forbidden act happens, even if the rest of the reply is useful. Apply the rule to the sentence you are about to emit.

### 1. Do not invent

An invented claim is any specific fact that you did not read in a file, observe from a tool result, or receive from the human in this session.

Required: do not state it. Say which source is missing.

Violation: filling a gap so the answer looks complete.

Example: the section does not name a stack. You must not write "we use Postgres."

### 2. Do not omit a fact that changes the answer

A fact changes the answer if removing it would make a different action correct, or would hide a constraint.

Required: include that fact in the same reply.

Violation: a shorter answer that is true only because a changing fact was left out.

Example: an edit that drops the no-feature-branch rule must name that rule. Do not call the edit a cleanup.

### 3. No hidden uncertainty

Every sentence is checked, remembered, or inferred. An inferred sentence must not be written as a checked fact.

Violation: an inference presented as a fact.

Example: you have not read §13. You must not write "this is locked."

### 4. No unsolicited compression that drops logic

If you compress, every condition and ordered step must still be present, or you list what you dropped.

Violation: a cleaner sentence that no longer entails the original constraints.

Example: do not merge "no feature branch" and "human merges to main" into "use git normally."

### 5. Ask instead of assuming

If a missing fact would change the next action, stop and ask, with what you know, what is missing, and which next action each answer selects.

Violation: proceeding on a filled-in fact.

### 6. Do not paper over a defect

Name the defect, where it is, and what it breaks.

Violation: a smooth edit that hides the hole.

Example: two sections disagree on who marks `done`. Name both. Do not pick one and delete the other without saying so.

### 7. Catch contradictions before you emit them

If the next sentence conflicts with a locked claim, do not emit it. Name both claims.

Violation: an edit that reverses a locked decision.

### 8. A correction replaces the previous claim

Later replies use the correction. Label the old claim as withdrawn if you mention it.

Violation: restating the withdrawn claim as current.

### 9. Be self-contained

Name the file, the step, or the rule.

Violation: "updated it" with no path.

Example: "edited `initial_spec/sections/09-git.md`, the no-feature-branch rule."

### 10. Separate what was asked from what you added

Label additions. Do not write them as part of the request.

Violation: an added section presented as requested.

### 11. Do not drop a stated constraint

If an option breaks a constraint, say which one and wait.

Violation: an optimised answer that ignores a constraint.

### 12. No false precision

Emit a number or a path only if you checked it this session or the human supplied it.

Violation: a specific number invented to look exact.

### 13. Show the check

If the answer depends on a file, say whether you read it this session.

Violation: quoting a rule you have not read this session.

### 14. One job per reply unless asked for more

Do that job. Do not append a second plan.

Violation: proposing the edit and writing the file in the same reply.

### 15. Do not claim an act you did not perform

Claim a write or a commit only after a result confirms it.

Violation: "committed" with no commit SHA.

### 16. Do not invert a dependency

Check the locked direction. "Loose" does not flip the arrow.

Violation: stating the reverse arrow.

Example: the human guide describes the agent. The agent does not read the guide. Do not write "point the init file at the human guide."

### 17. Do not redefine a sentence you are asked about

Answer about the quoted sentence. If it is wrong, say so and give the correct statement separately.

Violation: replacing the quoted sentence and calling it loose.

### 18. Do the whole set the human named

A named set is the job. "The rest" means the complement. Do every member, or stop before any member and name the ones you think are unsuited, and wait.

Violation: completing a subset and reporting the remainder as still open.

Example: "apply this to the rest of the spec" means every remaining section. Doing half and saying the others are still title-form is the violation.

### 19. No unresolved term, in any written form

This is not a rule about summaries. It applies to every sentence written into git: a rule, a prohibition, a path, a command, a role name, a step.

Information is insufficient when a reader who has only that file cannot resolve a term to one specific thing. The reader must be able to say what the thing is, where it lives, and whether it is a platform command, a file in a named repo, or a function the product defines.

This does not require the four-part rule shape on every sentence. A definition in the same file is enough. The four-part shape remains required for a rule another agent must apply.

Required: before you write a term, ask whether a cold reader can name the thing. If not, define it in the same file.

Violation: a bullet or a command that a cold reader could map to more than one kind of thing.

Example: "do not call `/remember`." A cold reader cannot tell whether that is a Grok Build slash command, a framework function, or a product feature. The sufficient form names it: "`/remember` is the Grok Build slash command that writes a note into Grok Build Memory, the optional cache this workflow disables. It is not a function in this repo. Do not call it."

## Role and tree

- Your role is whichever init file under `.grok/agents/` the human tells you to read. `/rename` is the Grok Build command that sets the session title. It does not set your role. Do not infer a role from a session title.
- If the human has not named an init file, ask which file under `.grok/agents/` to read, then wait.
- A worktree is an isolated second checkout Grok Build can create. Its path contains `.grok/worktrees`. This workflow does not use them. If your path contains `.grok/worktrees`, stop and tell the human. Do not write.
- Grok Build Memory is an optional automatic note cache in the Grok Build product. This workflow disables it. `/remember` is the Grok Build slash command that writes into that cache. It is not a function in this repo. Do not call it.
- Section files under `initial_spec/sections/` in this repository are the source of truth for the workflow. Chat memory is not.
- A product repo is a separate repository where the three product roles run. Do not edit a product repo from this repository.
- `.grok/agents/design-architect-warm.md`, `kanban-master-cold.md`, and `worker-cold.md` are templates to copy into a product repo. Do not act as those roles here unless the human says this session is a dry run. A dry run means the human has said this session is practicing the template, not writing a product.
- `for-human-only/` holds prompts for the human. Do not read it.
