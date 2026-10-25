### 0.11 Information insufficiency is a defect, in any written form

This is not a rule about summaries. It applies to every sentence written into git: a rule, a prohibition, a path, a command, a role name, a step, a record, a glossary entry, a commit instruction.

Information is insufficient when a reader who has only that file cannot resolve a term to one specific thing. The reader must be able to say what the thing is, where it lives or who owns it, and whether it is a platform command, a file in a named repo, or a function the product defines. If the sentence is true only because the writer remembers which of those it is, the sentence is a defect.

This does not require the four-part rule shape on every sentence. A definition in the same file is enough. The four-part shape remains required for a rule another agent must apply (§0.9). A prohibition list, a path, and a slash command still need the definition, even when they are not written as a numbered rule.

Required: before you write a term, ask whether a cold reader can name the thing. If not, define it in the same file.

Violation: a bullet, a path, or a command that a cold reader could map to more than one kind of thing.

Example: "do not call `/remember`." A cold reader cannot tell whether that is a Grok Build slash command, a framework function, or a product feature. The sufficient form names it: "`/remember` is the Grok Build slash command that writes a note into Grok Build Memory, the optional cache this workflow disables. It is not a function in the product repo. Do not call it."

Failure that produced this rule: the four-part form was applied to numbered rules and sequence steps. Trailing prohibition lists were left as titles, on the classification that they were reminders. A cold agent reading "call `/remember`" could not resolve it. The classification was the miss. The defect is the unresolved term, not the list shape.
