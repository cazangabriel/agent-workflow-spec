## 2. Purpose and problem

### 2.1 Purpose

Design the workflow for a team of agents running via Grok Build. The team's job is to build a software development project specified by the human. Example project types, not a closed list: public-facing website with a CMS; mobile store app; mobile game; Unity game; other software the user specifies.

### 2.4 Shared vocabulary between agents

Original brief: "We must take into account potential language barriers between agents with different skillsets."

Reading A — natural language: agents mixing English with another language, so the same requirement is not stably worded.
Reading B — specialist vocabulary: a DB-loaded agent and a UX-loaded agent use different words for the same thing, or the same word for different things. Tickets and living-knowledge then fail to bind.

The surrounding sentence named skillsets, so Reading B is the stronger parse. Reading A can still happen and is cheap to forbid.

Decided package: A + B + DDD extra (§2.5).

1. All tickets, living-knowledge files, commit messages, and agent-to-human questions are written in one project language (English, unless the user sets another in living-knowledge).
   Violation: a ticket body in a second language mid-project.
   Example: the project language is English. The worker does not write the ticket comment in Romanian.

2. Layer jargon is allowed inside a layer's detailed living-knowledge file.
   Violation: layer jargon in a ticket title that crosses roles.
   Example: `recipes-db.md` may say `integer not null`. The ticket title says "Add the recipe table", not "ALTER TABLE recipes ADD COLUMN".

3. Anything that crosses agents or roles — ticket titles, type abbreviations, shared terms for modules and layers, status words — must use the shared vocabulary in living-knowledge and in this spec. Do not invent a second name for an existing shared term.
   Violation: the worker calling the table `recipe_tbl` when living-knowledge says `recipes`.
   Example: living-knowledge says `recipes`. The migration creates `recipes`. Not `recipe` and not `recipe_tbl`.

4. If two skillsets would use different words for the same concept, the first writer picks one term, writes a one-line alias if needed, and later agents reuse that term. Do not translate between agent jargons in flight.
   Violation: the worker "translating" `cook_time` to `duration` because that is the UX word.
   Example: living-knowledge says `cook_time`. The worker uses `cook_time`. It does not rename it in the migration.

5. Precision differences are not resolved by chat. If a ticket is too vague to implement without guessing, the worker raises the unknown to the human instead of filling it (§0.5).
   Violation: the worker inventing the unit of `cook_time`.
   Example: the ticket does not state minutes or seconds. The worker comments and stops. It does not write `minutes`.

### 2.5 Ubiquitous Language (decided: A + B + DDD extra)

Ubiquitous Language is one language shared by the people and agents who talk about the domain, used in speech, tickets, living-knowledge, and code, with no private translation layer between business words and implementation words.

- A (natural language) is only a precondition. UL can be English or another language. Rule 1 in §2.4 sits under UL, not inside it.
- B (specialist vocabulary clash) is what UL is for. DB, UX, domain, and planning must not keep private synonyms for the same concept inside one bounded context.
- A+B is: pick one vehicle language (A) and enforce UL inside each bounded context (B).

Different bounded contexts may have different ubiquitous languages. That is allowed. An implicit, unwritten translation is not. If two contexts use the same English word for different things, living-knowledge at the boundary records the map. Layer specialists (DB vs UX) are not automatically different bounded contexts; often they are implementation dialects of one context and should collapse onto one term list.

Violation: two contexts using "order" for different things with no boundary map in living-knowledge.
Example: billing "order" and kitchen "order" are different. `abbreviations.md` or the boundary file says which is which. Agents do not guess.

Decided: adopt A + B + DDD extra. Not A alone (English with three words for order). Not B alone (shared terms, vehicle language can switch). Not one glossary for the whole app (smashes distinct contexts). Not DDD boxes with informal talk inside each box.

Default vehicle language is English unless the user sets another in living-knowledge. Layer implementation dialect may appear inside a detail file when it names a technical mechanism. Domain concepts in that same file still use the context's UL.
