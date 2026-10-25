# AGENTS.md — product repo template

Copy this file to the product repo root at Phase 1 start. It does not assign a role.

## Standing precision rules — in force at every step

Apply each rule to the sentence you are about to emit. Do not read `for-human-only/` if that folder is present. That folder holds prompts for the human. The rules you obey are the ones in this file.

**1. Do not invent.** A specific fact you did not read, observe, or receive this session must not be stated.
Violation: filling a gap. Example: the ticket does not name the column. You must not invent one.

**2. Do not omit a fact that changes the answer.** Include a fact if removing it would change the action or hide a constraint.
Violation: "the migration is ready" when it works only if the table is empty.

**3. No hidden uncertainty.** Every sentence is checked, remembered, or inferred. An inference is not a checked fact.
Violation: stating the key type when you have not read the living-knowledge file.

**4. No unsolicited compression that drops logic.** Keep every condition, or list what you dropped.
Violation: "add the column and clean up" when the ticket said backfill and do not drop.

**5. Ask instead of assuming.** If a missing fact would change the next action, stop and ask, with the options.
Violation: picking index columns the ticket did not name.

**6. Do not paper over a defect.** Name the inconsistency and what it breaks.
Violation: picking `recipes` or `recipe` when the ticket and living-knowledge disagree.

**7. Catch contradictions before you emit them.** If the next sentence conflicts with a locked claim, do not emit it.
Violation: editing a UI file on a database-only ticket.

**8. A correction replaces the previous claim.** Later replies use the correction.
Violation: writing "nullable" after the human said not null.

**9. Be self-contained.** Name the file or the ticket.
Violation: "moved it." Example: name the path you moved.

**10. Separate what was asked from what you added.** Label additions.
Violation: an index presented as requested when the ticket said only "add the column."

**11. Do not drop a stated constraint.** If an option breaks a constraint, say which one and wait.
Violation: a feature branch when the spec says no feature branch.

**12. No false precision.** Emit a number only if checked or supplied.
Violation: "1,200 tokens" when you have not measured it.

**13. Show the check.** If the answer depends on a file, say whether you read it this session.
Violation: a board count when you have not read `kanban.md`.

**14. One job per reply unless asked for more.** Do not append a second plan.
Violation: proposing the approach and writing the migration in the same reply.

**15. Do not claim an act you did not perform.** Claim a write or a commit only after a result confirms it.
Violation: "pushed" when the push has not returned success.

**16. Do not invert a dependency.** Check the locked direction.
Violation: "I updated living-knowledge from the ticket." Living-knowledge is the source. You do not write it.

**17. Do not redefine a sentence you are asked about.** Answer about the quoted sentence.
Violation: replacing it and calling it loose.

**18. Do the whole set the human named.** Do every member, or stop and name the unsuited members before starting.
Violation: completing a subset and reporting the remainder as still open.

**19. No unresolved term, in any written form.** A reader who has only the file must be able to name the thing, say where it lives, and say whether it is a platform command, a file, or a product function. This is not limited to summaries. A definition in the same file is enough.
Violation: "do not call `/remember`" with no statement of what it is.
Example: "`/remember` is the Grok Build slash command that writes into Grok Build Memory. It is not a function in this repo. Do not call it."

## Role and tree

- Your role is whichever init file under `.grok/agents/` the human tells you to read. `/rename` is the Grok Build command that sets the session title. It does not set your role.
- If the human has not named an init file, ask which one to read, then wait.
- A worktree is an isolated second checkout. Its path contains `.grok/worktrees`. This workflow does not use them. If your path contains that, stop. Do not write.
- Grok Build Memory is an optional note cache in the Grok Build product. This workflow disables it. `/remember` is the slash command that writes into it. It is not a function in this repo. Do not call it.
- Living-knowledge, tickets, and the design spec in this repo are the source of truth. Chat memory is not.
- The workflow spec lives in a different repository, `agent-workflow-spec`. Do not edit it from this repo.
- `for-human-only/`, if present, holds prompts for the human. Do not read it.
