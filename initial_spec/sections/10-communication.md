## 10. Communication rules

**Few agents.** Prefer few agents over many specialists, so the channel count stays small.
Violation: spawning a new specialist to answer a question the current agent can escalate.
Example: the worker is unsure. It comments and tells the human. It does not spawn a second worker.

**Substance only.** Keep talk on project, feature, and workflow substance.
Violation: a message that does not name a ticket, a file, a rule, or a decision.
Example: "looks good, let's go" is not a communication. "AM-10 approach agreed: add `cook_time integer not null`" is.

**No unbounded agent-to-agent threads.** If a thread becomes long, escalate to the human. Do not hope more tokens will converge it. A loop is a failure mode; break it by escalation.
Violation: a fourth message in a thread that has already repeated the same claim.
Example: two agents disagree on a column name for three messages. The next act is a comment to the human, not a fourth message.

**Shared vocabulary.** Shared-vocabulary risk between skillsets is in scope (§2.4). Use the ubiquitous language of the bounded context. Do not invent a synonym for a term already in living-knowledge.
Violation: the worker calling the table `recipe_tbl` when living-knowledge says `recipes`.

**Questions and statements carry their own context (§0.7).** A question must contain what is being asked, why it matters, the options, and what each option changes. A short label must be followed by what the parts are.
Violation: "clarify X" when X is only a heading the human cannot see.
Example: do not ask "which one?" Ask "which module for this ticket? AM means filename `AM-14-...`. REC means filename `REC-14-...`."

**One worker at a time (§5.3).** Worker-to-worker channels are out of scope until that constraint is lifted.
Violation: two Worker-cold sessions writing the same repo at the same time.

Human is in the loop for:

- requirements and planning with DesignArchitect-warm
- invoking KanbanMaster-cold ceremonies and spawn triggers
- choosing the next ticket from `open` (the worker may suggest from that list)
- approving or altering the implementation approach
- testing after pull
- receiving stop-rule reports from the worker and spawning KanbanMaster-cold (and DesignArchitect-warm via a TA if needed)
- long-thread escalation
- important choices and expensive-change estimates
- living-knowledge improvements an agent escalates
