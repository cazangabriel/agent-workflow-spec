### 0.9 Writing standard for every spec file and every init file

The reader of a section file, an init file, or `AGENTS.md` is a cold agent. That agent does not have the conversation that produced the sentence. A sentence that is clear only because the writer remembers the discussion is a defect.

Required shape for a rule:

1. Define the term. Say what counts as an instance of it.
2. State the required act.
3. State the violation, so a tidy rewrite cannot hide it.

Forbidden:

- A slogan that needs this conversation to interpret (“be self-contained”, “wait”, “first”, “propose” with no step number and no prior-step condition).
- A bag of duties with no order. If the duties are a workflow, number the steps and say what must already be true before each step starts.
- A short label (“extra hop”, “module card”, “mini Phase 1”) used without the definition in the same place.

This standard applies to every later edit of this spec, not only to the precision rules.
