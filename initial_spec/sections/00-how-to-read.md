## 0. How to read and maintain this document

### 0.1 What this file is

The original user message mixed: conversation protocol, product intent, team design, knowledge architecture, ticket/git rules, and four “god-level / dogma” rules. This rewrite keeps those contents and groups them by cohesion so a later agent can use the file without private conversational context.

### 0.2 Binding rule for every later editor of this file (dogma)

This specification must make sense in its own universe. Do not keep key binding information only in an agent’s internal context. If a term, constraint, sequence, or detail is a component that joins other concepts together — or is itself a requirement — it belongs in this document.

Balance, not two extremes:

- Do not treat every word as sacred.
- Do not drop a requirement-shaped detail because it feels “already known” internally.

Worked warning from the brief (must stay in the spec): if the app must show butterflies, and the user later specifies they are blue, “blue” is part of the requirement. Losing “blue” in conversation, or deleting it later because “the model already knows the butterflies are blue,” is a defect.

### 0.3 Edit policy for this file and for later workflow/spec files (dogma)

Unsolicited compression is forbidden when it removes meaning, edges, or details.

Allowed:

- Inline edits that keep sections coherent.
- Merging duplicated material when all relevant scenario-bound details are preserved.
- Compression that does not lose important details.

Forbidden:

- Trigger-happy “improvements” of sentences the editor already rewrote many times, if the result only makes sense inside the editor’s private context.
- After N passes, a document whose original meaning is diluted.
- The opposite failure mode: refusing to edit existing text and only appending addenda / apply-diffs such as “v13.4.6: section 7 paragraph 3 — add the word backend.” That produces an unreadable spec.

High cohesion / low coupling applies to the spec itself:

- Topic X should not be scattered across seven unrelated places without a reason.
- A cross-cutting section (for example Security Considerations) does not require every topic section to duplicate a full security subsection. Put shared cross-cutting rules in the cross-cutting section; put topic-specific exceptions next to the topic.

### 0.4 Merge rule when two passages share a stem concept (dogma)

If one place has concept `A-B-x-y` and another has `A-B-t-w` (where `x,y` and `t,w` are details bound to `A-B` in different contexts):

- Do not merge `A-B` by keeping only `x-y` or only `t-w`.
- If both detail sets are relevant in different scenarios, the merged section must carry `A-B-x-y-t-w` (or an equivalent complete encoding of both scenarios).
- If `y` is the same as `t`, the merge may look like `A-B-x-y-w`. Use propositional logic; preserve relevant details.

### 0.5 Uncertainty rule for every agent that touches this work (dogma)

Unknowns and uncertainty are the enemies of correct estimation. Hidden unknowns make the human’s estimates wrong and later look like the agent “lied by being certain.”

Required behaviour:

- State uncertain or unknown variables explicitly.
- “I don’t know; I would suggest A, B, or C” is preferred to a confident answer that silently contains variables that could drop confidence.
- When not sure, raise it with the user.
- Do not replace unknowns with assumptions and then implement against those assumptions.

This rule is first-class product behaviour, not style advice.

### 0.6 Skills assumed of the editor of this rewrite

The brief assigned the rewriting agent these skills and required they be used: up-to-date expert knowledge of AI agents; mathematics for precision of thought; expert propositional logic, used to avoid inconsistencies, including the merge rule in §0.4; expert software engineering; expert software architecture; exceptional abstract thinking; expert project management, line management, and product management; expert specification drafting.

### 0.7 Questions and statements to the human must carry their own context

An agent asking the human a question must make that question understandable from the question itself, without relying on the agent’s private memory of an earlier short label.

Required contents of a question: what is being asked, in full; why the answer matters; the options, if any, and what would change for each; any quote of the source phrase being disambiguated.

Forbidden: “clarify X” when X is only a heading for a longer idea the human cannot see.

The same duty applies to statements, not only questions. A short label must be followed, in the same breath, by what the parts are.

### 0.8 Catching semantic defects without waiting for the human

An editor of this spec must run the following pass after any material change:

1. Role × artifact matrix. For every artifact that can be written, list who is allowed to write it and which skills that writer is required to have. If the writer lacks the skills the artifact needs, that assignment is a defect.
2. Stereotype inversion. For every job title, ask whether the brief-meaning and the street-meaning disagree. If they do, pick one and write it down.
3. Two-reader test. Read each permission as if a later agent has only this file.
4. State-vocabulary test. Every board column and filename token must map to one state list.
5. Stop-rule test. Whenever a worker is told both “continue” and “raise unknowns,” write the stop condition that chooses between those two.
6. Cost-relative test. Words like cheap or expensive are meaningless without the planning budget they are relative to.

### 0.9 Writing standard for every spec file, init file, and prompt written for another agent

The reader is a cold agent. That agent does not have the conversation that produced the sentence. A sentence that is clear only because the writer remembers the discussion is a defect.

Required shape for a rule written for another agent:

1. Define the term. Say what counts as an instance of it.
2. State the required act.
3. State the violation, so a tidy rewrite cannot hide it.
4. Anchor it with one concrete example. The example names the inputs, the forbidden output, and the required output. A title with no example is not a rule.

Forbidden: a slogan that needs this conversation to interpret; a bag of duties with no order; a short label used without the definition in the same place; compressing a rule that was discussed with examples into a one-line title.

This standard applies to every later edit of this spec, to init files, to `AGENTS.md`, to human guides, and to any starter prompt written for a session that will not have this conversation.

### 0.10 Do the whole set the human named

A named set is the job. "The rest" means the complement of what is already done. A list means every item on the list.

Required: do every member of the set. If a member looks unsuited to the form being applied, stop before any member and name that member, say why, and wait. Do not subset the set by your own cost or priority judgment.

Violation: completing a subset and reporting the remainder as "still open" or "next", as if the subset were the job.

Example: the human says "apply this form to the rest of the spec." Doing §3–§12 and writing "§14 is still title-form" is the violation. Either do every remaining section, or before starting name §13 and §16 as records and ask.

### 0.11 Information insufficiency is a defect, in any written form

This is not a rule about summaries. It applies to every sentence written into git: a rule, a prohibition, a path, a command, a role name, a step, a record, a glossary entry, a commit instruction.

Information is insufficient when a reader who has only that file cannot resolve a term to one specific thing. The reader must be able to say what the thing is, where it lives or who owns it, and whether it is a platform command, a file in a named repo, or a function the product defines.

This does not require the four-part rule shape on every sentence. A definition in the same file is enough. The four-part shape remains required for a rule another agent must apply (§0.9). A prohibition list, a path, and a slash command still need the definition.

Required: before you write a term, ask whether a cold reader can name the thing. If not, define it in the same file.

Violation: a bullet, a path, or a command that a cold reader could map to more than one kind of thing.

Example: "do not call `/remember`." A cold reader cannot tell whether that is a Grok Build slash command, a framework function, or a product feature. The sufficient form names it: "`/remember` is the Grok Build slash command that writes a note into Grok Build Memory, the optional cache this workflow disables. It is not a function in the product repo. Do not call it."

### 0.12 Before you reply: check the artifact, not the intention

This is enforcement of the rules above. It is not a new prohibition. A rule that is not run on the work about to be handed back does not catch.

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
Example: the human asked for a section edit. You do not reply with "1. Dropped part: rule 1 still has Required. 3. Commit abc123." You reply with the edit. If rule 1 lost `Required:`, you say "check failed: rule 1 has no Required. I am not sending that write."

Stop: do not send the work if one check fails.

Failure that produced the log ban: the writer pasted the five checks, the commit SHAs, and the file sizes into the reply. The human called it a debugger log. The check is internal. The reply is the work.
