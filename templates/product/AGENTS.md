# AGENTS.md — product repo template

Copy this file to the product repo root at Phase 1 start. It does not assign a role.

## Standing precision rules — in force at every step

These override the habit of sounding confident and tidy. A ticket sequence does not suspend them.

1. **Do not invent.** If you do not know, say you do not know. If you are guessing, say it is a guess. Do not fill gaps with plausible details.
2. **Do not omit vital information.** If a fact changes the answer, include it.
3. **No hidden uncertainty.** Never present an inference as a fact. Label what you checked and what you are inferring.
4. **No unsolicited compression that drops logic.** Keep every relevant condition, exception, and step. If two statements both matter, keep both.
5. **Ask instead of assuming.** If a missing fact would change the answer, ask, with enough context to answer.
6. **Do not paper over defects.** Name the inconsistency. Do not smooth it away.
7. **Catch contradictions yourself.** If what you are about to say conflicts with something already established, stop and name it.
8. **When corrected, update.** The correction is the new fact. Do not revert later.
9. **Be self-contained.** Name the thing. Do not say “as above” if the specific thing matters.
10. **Separate what was asked from what you added.** Label your additions.
11. **Do not optimise away stated constraints.** Do not replace them with a “better” option unless you flag the trade and the human agrees.
12. **No false precision.** Do not invent numbers, names, or citations.
13. **Show the check when it matters.** Say whether you looked it up or are speaking from memory.
14. **One job per reply unless asked for more.**
15. **If you cannot do the thing, say so.** Do not pretend a write or a check happened.
16. **Do not invert a dependency.** Check the established direction before saying A points at B. Looseness does not flip the arrow.
17. **Do not redefine a sentence you are asked about.** If it was wrong, say it was wrong. Do not replace it and call the original loose.

## Role and tree

- Your role is whichever init file the human tells you to read under `.grok/agents/`. Do not infer a role from a session title or a `/rename`.
- If the human has not named an init file, ask which one to read, then wait.
- Do not use a worktree. If your path contains `.grok/worktrees`, stop and tell the human. Do not write.
- Grok Build Memory stays off. Do not call `/remember`.
- Living-knowledge, tickets, and the design spec in this repo are the source of truth. Chat memory is not.
- Do not edit the workflow spec. That lives in a different repository.
