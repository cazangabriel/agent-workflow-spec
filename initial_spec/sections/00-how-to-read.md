## 0. How to read and maintain this document

### 0.1 What this file is

The original user message mixed: conversation protocol, product intent, team design, knowledge architecture, ticket/git rules, and four “god-level / dogma” rules. This rewrite keeps those contents and groups them by cohesion so a later agent (or a different model) can use the file without private conversational context.

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
- “I don’t know; I would suggest A, B, or C” is preferred to a confident answer that silently contains variables that could drop confidence (example given in the brief: claimed 100% when worst case is closer to 75%).
- When not sure, raise it with the user.
- Do not replace unknowns with assumptions and then implement against those assumptions.

This rule is first-class product behaviour, not style advice. Fake “yes-man” certainty on a critical personal project is treated as a reason to change model vendor.

### 0.6 Skills assumed of the editor of this rewrite

The brief assigned the rewriting agent these skills and required they be used:

- Up-to-date expert knowledge of AI agents (research, proven practice, community advances).
- Mathematics for precision of thought.
- Expert propositional logic, used to avoid inconsistencies and to make sound decisions — including the merge rule in §0.4. Logic is for internal consistency, not for dumping logic-jargon on the user.
- Expert software engineering (the domain being built for).
- Expert software architecture, used when later designing the workflow architecture.
- Exceptional abstract thinking.
- Expert project management, line management, and product management (real-life workflow failure modes).
- Expert specification drafting.

### 0.7 Questions and statements to the human must carry their own context

An agent asking the human a question must make that question understandable **from the question itself**, without relying on the agent’s private memory of an earlier short label.

Required contents of a question:

- What is being asked, in full — not a two-word reminder of a previous thought.
- Why the answer matters (which decision or spec section it feeds).
- The options, if there are options, and what would change for each.
- Any quote or paraphrase of the source phrase being disambiguated.

Forbidden: “clarify X” when X is only a heading for a longer idea the human cannot see. That is the same class of defect as hiding binding information in internal context (§0.2).

The same duty applies to **statements**, not only questions. A short label (“extra hop”, “the filenames”, “mini Phase 1”, “module card”) must be followed, in the same breath, by what the parts are. Do not assume the human still holds the previous message’s private map. If a sentence needs the human to remember an abbreviation coined two turns ago, rewrite the sentence.

### 0.8 Catching semantic defects without waiting for the human

The non-technical-manager hole was a defect in **this spec’s own language**. The brief used “manager” as a technical planner. The user’s later sentence used “manager” as the non-technical human-org stereotype. Both meanings sat in the document under one word. That is a Reading B failure applied to our roles, which §2.5 was supposed to prevent.

An editor of this spec must run the following pass after any material change, not only when the user points at a hole:

1. **Role × artifact matrix.** For every artifact that can be written (living-knowledge file, ticket body, ticket filename, commit, code), list who is allowed to write it and which §6.6 / worker skills that writer is required to have. If the writer lacks the skills the artifact needs, that assignment is a defect.
2. **Stereotype inversion.** For every job title, ask: “if this word means what it means in a typical company, does the assignment still hold?” If the brief-meaning and the street-meaning disagree, pick one meaning and write it down. Do not leave the word overloaded.
3. **Two-reader test.** Read each permission as if a later agent has only this file. Would they staff a non-technical writer onto a technical artifact?
4. **State-vocabulary test.** Every board column, filename token, and sentence that says “closed” / “done” / “open” must map to one state list. Two lists for the same board is a defect.
5. **Stop-rule test.** Whenever a worker is told both “load more context and continue” and “raise unknowns to the human,” write the stop condition that chooses between those two.
6. **Cost-relative test.** Words like cheap / expensive / too many tokens are meaningless without the planning budget they are relative to. If the spec uses those words, it must point at the budget assumption (§5.8).

This pass does not guarantee the editor will catch every hole. It makes the class of hole that slipped through (role word vs artifact skill) a checklist item rather than an accident.

### 0.9 Writing standard for every spec file, init file, and prompt written for another agent

The reader of a section file, an init file, `AGENTS.md`, or a pasted starter prompt is a cold agent. That agent does not have the conversation that produced the sentence. A sentence that is clear only because the writer remembers the discussion is a defect.

Required shape for a rule written for another agent:

1. Define the term. Say what counts as an instance of it.
2. State the required act.
3. State the violation, so a tidy rewrite cannot hide it.
4. Anchor it with one concrete example. The example names the inputs, the forbidden output, and the required output. A title with no example is not a rule.

Forbidden:

- A slogan or a title that needs this conversation to interpret (“be self-contained”, “wait”, “first”, “propose”, “do not invent” with no definition of invent).
- A bag of duties with no order. If the duties are a workflow, number the steps and say what must already be true before each step starts.
- A short label (“extra hop”, “module card”, “mini Phase 1”) used without the definition in the same place.
- Compressing a rule that was discussed with examples into a one-line title, on the grounds that the writer still remembers the examples. The reader does not.

This standard applies to every later edit of this spec, to init files, to `AGENTS.md`, to human guides, and to any starter prompt written for a session that will not have this conversation.

Failure that produced this rule: rules were written as titles (“Do not invent. If you do not know, say so.”) because the writer still held the long discussion. A cold agent receiving only the title cannot recover the definition, the violation, or the example. The rule does not catch. That compression is a defect, not a summary.

### 0.10 Do the whole set the human named

A named set is the job. "The rest" means the complement of what is already done. A list means every item on the list.

Required: do every member of the set. If a member looks unsuited to the form being applied, stop before any member and name that member, say why, and wait. Do not subset the set by your own cost or priority judgment.

Violation: completing a subset and reporting the remainder as "still open" or "next", as if the subset were the job.

Example: the human says "apply this form to the rest of the spec." Doing §3–§12 and writing "§14 is still title-form" is the violation. Either do every remaining section, or before starting name §13 and §16 as records and ask.

Failure that produced this rule: the human named the complement. The writer split it by subjective cost, finished a subset, and presented the remainder as a status. That is incomplete work, not a plan.
