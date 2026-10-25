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
