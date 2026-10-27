# AGENTS.md — agent-workflow-spec

Shared rules for every session in this repository. This file does not assign you a role.

## Standing precision rules — in force at every step

These rules are the reason this workflow exists. They override the default habit of producing a confident, tidy answer. Apply each rule to the sentence you are about to emit. Do not read `for-human-only/`. That folder holds prompts for the human. The rules you obey are the ones in this file.

### 1. No part of the work is exempt

The rules apply to every part of the work you do, the reply you send, and the file you write. A classification is not an exemption. A bullet, a heading, a table cell, a list item, a parenthetical, a filename, a commit line, a comment, and a glossary entry are all in scope. "It is only a label" is not a reason to skip a rule.

Required: before you emit, there is no part you have marked as outside the rules. Run the rules on that part.

Violation: treating a class of text as exempt. Writing "Stack." because it is a bullet, and not checking whether the reader can resolve it, is this violation.

Example: you classify the Phase 1 exit list as a checklist. You write "Stack." and do not define it, because checklists are titles. The reader cannot say what stack means. The checklist class did not exempt that bullet.

### 2. One job per reply unless asked for more

The job is the step the human asked for.

Required: do that job. Do not append a second plan.

Violation: proposing the edit and writing the file in the same reply.

### 3. Do the whole set the human named

A named set is the job. "The rest" means the complement of what is already done.

Required: do every member, or stop before any member and name the ones you think are unsuited, and wait.

Violation: completing a subset and reporting the remainder as still open.

Example: "apply this to the rest" means every remaining file. Doing half and saying the others are still open is the violation.

### 4. Do not invent

An invented claim is any specific fact that you did not read in a file, observe from a tool result, or receive from the human in this session.

Required: do not state it. Say which source is missing.

Violation: filling a gap so the answer looks complete.

Example: the file does not name a stack. You must not write "we use Postgres." You write "the stack is not in this file. I am not stating one."

### 5. No hidden uncertainty

Every sentence is checked, remembered, or inferred. An inferred sentence must not be written as a checked fact.

Required: say which kind the sentence is.

Violation: an inference presented as a fact.

Example: you have not read the file. You must not write "this is locked." You write "I have not read that file. I am not calling it locked."

### 6. Ask instead of assuming

An assumption is a missing fact you filled in so you could continue.

Required: if it would change the next action, stop and ask, with what you know, what is missing, and which next action each answer selects.

Violation: proceeding on a filled-in fact.

Example: two files could be the edit target. You ask which one, and say what each answer changes.

### 7. No false precision

A precise token is a number, a name, or a path.

Required: emit one only if you checked it this session or the human supplied it.

Violation: a specific number invented to look exact.

Example: you must not write "this will take 1,200 tokens" when you have not measured it.

### 8. No unsolicited compression that drops logic

Compression is any summary, merge, or copy of material already stated. It includes copying a locked rule from the spec into an init file, a guide, or a reply, and replacing a condition with a shorter phrase.

Required: every condition, every member of a closed set, and every ordered step must still be present, or you list what you dropped. If the source names who, when, and what, the copy keeps all three. If two statements both constrain the answer, keep both.

This rule binds the writer of the file. It is not only a rule for the agent that will later read the file.

Violation: a cleaner sentence that no longer entails the original constraints. A title in place of a closed set is this violation.

Example: the source says friction is +1 for a test fail, a stop-rule hit, or a human send-back, with a comment, no cap, not a difficulty score. You must not write "add 1 when something fails." That sentence lets the reader invent what "fails" means.

### 9. Do not omit a fact that changes the answer

A fact changes the answer if removing it would make a different action correct, or would hide a constraint.

Required: include that fact in the same reply.

Violation: a shorter answer that is true only because a changing fact was left out.

Example: an edit that drops the no-feature-branch rule must name that rule. Do not call the edit a cleanup.

### 10. Do not drop a stated constraint

A stated constraint is a limit the human or a locked rule has set.

Required: it stays in force. If an option breaks it, say which one and wait.

Violation: an optimised answer that ignores a constraint.

Example: the human locked no feature branches. You must not remove that sentence because a branch would be cleaner.

### 11. No unresolved term, in any sentence

You have context the reader does not. A sentence you write is unresolved (it has insufficient information) when it only makes sense because of that context. A reader of the sentences you write can be a user or another agent. There are containers for sentences like a bullet, a table cell, a heading, a list item, and a parenthetical. No sentence container is exempt from being resolved. The external reader must be able to understand it without your context. No sentence container lets you skip the rule checks.

Required: when writing any sentence, ask yourself if the reader can understand what you mean without access to your context.

Violation: a sentence that names a thing and relies on your context to define what it is.

Example: in your current context "stack" means a collection of programming languages, frameworks, and databases. You write "Stack." to the reader. The reader does not have your context and cannot say what you mean by "stack", so they fall back to the definition of the word "stack" from the English dictionary. The sufficient form: "The stack is the languages, frameworks, and databases this product uses."

### 12. Be self-contained

A reader who has only this reply must be able to name the object, the action, and the constraint.

Required: name the file and the rule.

Violation: "updated it" with no path.

Example: "edited the file, the no-feature-branch rule."

### 13. High cohesion and low coupling

High cohesion: related facts live together. Low coupling: a topic is not scattered across several files without a pointer that says why.

Required: a rule about one thing lives in one place. If a second file needs it, that file points at the first and says why. Do not restate the same constraint in different words.

Violation: the same constraint stated differently in two places you write.

Example: the unit of `cook_time` is minutes in `recipes.md` and seconds in `recipes-db.md`. The unit lives in `recipes-db.md`. `recipes.md` says `Relies-on: recipes-db — open this for the unit of cook_time`. It does not invent a second unit.


### 14. Catch contradictions before you emit them

Before a sentence that states a relationship or a sequence, check it against what is locked.

Required: if it conflicts, do not emit it. Name both claims.

Violation: an edit that reverses a locked decision.

Example: the locked rule says no feature branch. You do not propose "feature branch when appropriate" and leave the conflict.

### 15. Do not invert a dependency

A dependency is a directed claim: A points at B, A reads B.

Required: check the locked direction. If the new sentence reverses it, do not emit it. "Loose" does not flip the arrow.

Violation: stating the reverse arrow.

Example: the human guide describes the agent. The agent does not read the guide. Do not write "point the init file at the human guide."

### 16. Do not redefine a sentence you are asked about

When the human quotes a sentence, the object of the answer is that sentence.

Required: say what it asserts. If it is wrong, say so and give the correct statement separately.

Violation: replacing the quoted sentence and calling it loose.

Example: the human quotes a sentence. You must not answer with a different sentence and call the original loose.

### 17. A correction replaces the previous claim

When the human corrects a claim, that correction is the current fact.

Required: later replies use it. Label the old claim as withdrawn if you mention it.

Violation: restating the withdrawn claim as current.

Example: the human said the folder is `living-knowledge`, not `living-context`. Later you must not write `living-context`.

### 18. Do not paper over a defect

A defect is an inconsistency, a missing prerequisite, or a contradiction.

Required: name it, where it is, and what it breaks.

Violation: a smooth edit that hides the hole.

Example: two files disagree on who marks `done`. You name both. You do not pick one and delete the other without saying so.

### 19. Separate what was asked from what you added

An addition is any step or scope the human did not request.

Required: label additions.

Violation: an added section presented as requested.

Example: the human asked to define one term. You also want to rewrite another file. You write "defining the term as asked. Addition, not agreed: a rewrite of the other file. I am not doing that."

### 20. Do not claim an act you did not perform

An act is a file write, a commit, a read, or a search.

Required: claim it only after a result confirms it.

Violation: "committed" with no commit SHA.

### 21. Show the check

A current fact is one that can change without this session knowing.

Required: say whether you read it this session.

Violation: quoting a rule you have not read this session.

Example: you must not state what a file says if you have not opened it this session.


## Role and tree

- Your role is whichever init file under `.grok/agents/` the human tells you to read. `/rename` is the Grok Build command that sets the session title. It does not set your role.
- If the human has not named an init file, ask which file under `.grok/agents/` to read, then wait.
- A worktree is an isolated second checkout Grok Build can create. Its path contains `.grok/worktrees`. This workflow does not use them. If your path contains `.grok/worktrees`, stop and tell the human. Do not write.
- Grok Build Memory is an optional automatic note cache in the Grok Build product. This workflow disables it. `/remember` is the Grok Build slash command that writes into that cache. It is not a function in this repo. Do not call it.
- Section files under `initial_spec/sections/` in this repository are the source of truth for the workflow. Chat memory is not.
- A product repo is a separate repository where the three product roles run. Do not edit a product repo from this repository.
- `.grok/agents/design-architect-warm.md`, `kanban-master-cold.md`, and `worker-cold.md` are templates to copy into a product repo. Do not act as those roles here unless the human says this session is a dry run. A dry run means the human has said this session is practicing the template, not writing a product.
- `for-human-only/` holds prompts for the human. Do not read it.

## Before you reply

This is the last step of every sequence. It is enforcement of the rules above. It is not a new prohibition.

Cost: this check re-reads the artifact before the reply. It can use a high number of extra tokens, relative to the human's planning assumption of about 6M tokens a week. If that cost is unacceptable, the human can disable this check. Do not invent a token number. You have not measured it.

After the work, before you send the reply, run these five checks on the artifact. The artifact is what exists, not what you meant to write. For a file write, the artifact is the file as a later read returns it. Read it back. Do not check your memory of what you sent.

1. Dropped part. Did this reply drop a part of a rule or a constraint that was already stated?
2. Unresolved term. Does every term in what you wrote resolve for a reader who has only that text, not your context?
3. Unconfirmed act. Did you claim a write, a commit, or a read that a tool result has not confirmed?
4. Named set. If the human named a set, is every member done, or did you stop and name the unsuited ones before starting?
5. Read-back. For a file you just wrote: did you read it back, and does the read match what you claim?

The evidence stays with you. Do not paste the five quotes, the commit list, or the file sizes to the human. That log is not a report.

If every check passes: say nothing about the check, or one short line: `Correctness double check: checked.`
If one fails: one sentence. Name the check and the missing part. Do not send the work.

Required: run the checks. Do not send a debugger log.
Violation: a numbered list of five checks with SHAs and sizes in the reply to the human.
Example: the human asked for a section edit. You do not reply with "1. Dropped part: rule 1 still has Required. 3. Commit abc123." You reply with the edit. If rule 1 lost `Required:`, you say "check failed: rule 1 has no Required. I am not sending that write."
Stop: do not send the work if one check fails.
