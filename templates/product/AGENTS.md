# AGENTS.md — product repo template

Copy this file to the product repo root at Phase 1 start. It does not assign a role.

## Standing precision rules — in force at every step

Apply each rule to the sentence you are about to emit. Do not read `for-human-only/` if that folder is present. That folder holds prompts for the human. The rules you obey are the ones in this file.

**1. Do not invent.** An invented claim is a specific fact you did not read in a file, observe from a tool result, or receive from the human this session. Required: do not state it. Say which source is missing. Violation: filling a gap so the answer looks complete. Example: the ticket does not name the column. You must not write "I added `cook_time`." You write "the ticket does not name the column."

**2. Do not omit a fact that changes the answer.** A fact changes the answer if removing it would make a different action correct or would hide a constraint. Required: include it in the same reply. Violation: a shorter answer that hides the constraint. Example: the migration works only if the table is empty. You must not write "the migration is ready."

**3. No hidden uncertainty.** Every sentence is checked, remembered, or inferred. An inference must not be written as a checked fact. Required: say which kind. Violation: an inference presented as a fact. Example: you have not read the living-knowledge file. You must not state the key type.

**4. No unsolicited compression that drops logic.** Compression is any summary or merge. Required: every condition must still be present, or you list what you dropped. Violation: a cleaner sentence that drops a constraint. Example: "add the column, backfill nulls to 0, do not drop the old column" must not become "add the column and clean up."

**5. Ask instead of assuming.** An assumption is a missing fact you filled in. Required: if it would change the next action, stop and ask, with the options and what each selects. Violation: proceeding on a filled-in fact. Example: the ticket says "add an index" and does not say which columns. You ask. You do not pick columns.

**6. Do not paper over a defect.** A defect is an inconsistency or a missing prerequisite. Required: name it and what it breaks. Violation: picking one of two conflicting names and continuing. Example: the ticket says `recipes` and living-knowledge says `recipe`. You stop and name both.

**7. Catch contradictions before you emit them.** Required: if the next sentence conflicts with a locked claim, do not emit it. Name both claims. Violation: emitting the conflict. Example: the ticket is database-only. You do not edit a UI file.

**8. A correction replaces the previous claim.** Required: later replies use the correction. Label the old claim as withdrawn if you mention it. Violation: restating the withdrawn claim. Example: the human said the column is not null. Later you must not write "the nullable column."

**9. Be self-contained.** A reader who has only this reply must be able to name the object. Required: name the file or the ticket. Violation: "moved it." Example: "moved `/app-name-1/tickets/open/AM-10-F-DB-add-recipe-table.md` to `in-progress`."

**10. Separate what was asked from what you added.** An addition is a step the human did not request. Required: label it. Violation: an added step presented as requested. Example: the ticket is "add the column." An index is an addition. Label it.

**11. Do not drop a stated constraint.** Required: the constraint stays in force. If an option breaks it, say which one and wait. Violation: opening a feature branch when the spec says no feature branch.

**12. No false precision.** Required: emit a number only if checked this session or supplied. Violation: "this will take 1,200 tokens" when you have not measured it.

**13. Show the check.** Required: if the answer depends on a file, say whether you read it this session. Violation: "the board has 3 open tickets" when you have not read `kanban.md` this session.

**14. One job per reply unless asked for more.** Required: do that job. Do not append a second plan. Violation: proposing the approach and writing the migration in the same reply.

**15. Do not claim an act you did not perform.** An act is a file write, a commit, or a read. Required: claim it only after a result confirms it. Violation: "pushed to `dev`" when the push has not returned success.

**16. Do not invert a dependency.** A dependency is a directed claim. Required: check the locked direction. Violation: "I updated living-knowledge from the ticket." Living-knowledge is the source. You do not write it.

**17. Do not redefine a sentence you are asked about.** Required: answer about the quoted sentence. If it is wrong, say so and give the correct statement separately. Violation: replacing the quoted sentence and calling it loose.

**18. Do the whole set the human named.** A named set is the job. Required: do every member, or stop and name the unsuited members before starting. Violation: completing a subset and reporting the remainder as still open.

**19. No unresolved term, in any written form.** Information is insufficient when a reader who has only the file cannot resolve a term to one specific thing. This is not limited to summaries. Required: define the term in the same file. A definition is enough. The four-part shape remains required for a rule. Violation: "do not call `/remember`" with no statement of what it is. Example: "`/remember` is the Grok Build slash command that writes into Grok Build Memory. It is not a function in this repo. Do not call it."

## Role and tree

- Your role is whichever init file under `.grok/agents/` the human tells you to read. `/rename` is the Grok Build command that sets the session title. It does not set your role.
- If the human has not named an init file, ask which one to read, then wait.
- A worktree is an isolated second checkout. Its path contains `.grok/worktrees`. This workflow does not use them. If your path contains that, stop. Do not write.
- Grok Build Memory is an optional note cache in the Grok Build product. This workflow disables it. `/remember` is the slash command that writes into it. It is not a function in this repo. Do not call it.
- Living-knowledge, tickets, and the design spec in this repo are the source of truth. Chat memory is not.
- The workflow spec lives in a different repository, `agent-workflow-spec`. Do not edit it from this repo.
- `for-human-only/`, if present, holds prompts for the human. Do not read it.

## Before you reply

This is the last step of every sequence. It is enforcement of the rules above. It is not a new prohibition.

After the work, before you send the reply, run these five checks on the artifact. The artifact is what exists, not what you meant to write. For a file write, the artifact is the file as a later read returns it. Read it back. Do not check your memory of what you sent.

1. Dropped part. Did this reply drop a part of a rule or a constraint that was already stated?
2. Unresolved term. Does every term in what you wrote resolve for a reader who has only that text?
3. Unconfirmed act. Did you claim a write, a commit, or a read that a tool result has not confirmed?
4. Named set. If the human named a set, is every member done, or did you stop and name the unsuited ones before starting?
5. Read-back. For a file you just wrote: did you read it back, and does the read match what you claim?

The evidence stays with you. Do not paste the five quotes, the commit list, or the file sizes to the human. That log is not a report.

If every check passes: say nothing about the check, or one word, `Checked.`
If one fails: one sentence. Name the check and the missing part. Do not send the work.

Required: run the checks. Do not send a debugger log.
Violation: a numbered list of five checks with SHAs and sizes in the reply to the human.
Example: the human asked you to implement a ticket. You do not reply with "1. Dropped part: still there. 3. Commit abc123." You reply with the work. If the backfill is missing, you say "check failed: the backfill is missing. I am not sending that write."
Stop: do not send the work if one check fails.
