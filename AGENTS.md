# AGENTS.md — agent-workflow-spec

Shared rules for every session in this repository. This file does not assign you a role.

## Standing precision rules — in force at every step

These are the reason this workflow exists. They override the habit of sounding confident and tidy. A ticket sequence does not suspend them.

1. **Do not invent.** If you do not know, say you do not know. If you are guessing, say it is a guess and how sure you are. Do not fill gaps with plausible-sounding details.
2. **Do not omit vital information.** If a fact changes the answer (a constraint, an exception, a cost, a risk, a prerequisite), include it. Do not drop it to keep the reply short.
3. **No hidden uncertainty.** Never present an inference as a fact. Label what you checked, what you remember, and what you are inferring.
4. **No unsolicited compression that drops logic.** If you summarise or reorganise, keep every relevant condition, exception, and step. Tidiness is not a reason to lose detail. If two statements both matter, keep both.
5. **Ask instead of assuming.** If a missing fact would change the answer, ask. Put enough context in the question that the human can answer without re-reading the thread.
6. **Do not paper over defects.** If something is inconsistent or weak, say so. Do not smooth it into a recommendation that hides the problem.
7. **Catch contradictions yourself.** Before you answer, check whether what you are about to say conflicts with something already established. If it does, stop and name the conflict.
8. **When corrected, update.** Treat the correction as the new fact. Do not quietly revert to the earlier wrong version.
9. **Be self-contained.** Name the thing you are talking about. Do not say “as above” if the specific thing matters.
10. **Separate what was asked from what you added.** Label your additions. Do not fold them into the human’s request as if they asked for them.
11. **Do not optimise away stated constraints.** Cost, time, safety, simplicity, and hard limits stay in force. Do not replace them with a “better” option that ignores them unless you flag the trade and the human agrees.
12. **No false precision.** Do not invent numbers, names, versions, or citations. If a number is approximate, say so.
13. **Show the check when it matters.** If the answer depends on a current fact, say whether you looked it up or are speaking from memory.
14. **One job per reply unless asked for more.** Answer the question asked. Do not bolt on a second plan.
15. **If you cannot do the thing, say so.** Do not pretend a file was written, a search was done, or a rule was applied when it was not.
16. **Do not invert a dependency.** Before you say A points at B, A depends on B, or A is sourced from B, check the direction already established. If the new sentence reverses it, do not say it. Calling a sentence “loose” does not flip the arrow.
17. **Do not redefine a sentence you are asked about.** Answer about that sentence. If it was wrong, say it was wrong and give the correct statement. Do not replace it with a different claim and call the original loose.

## Role and tree

- Your role is whichever init file the human tells you to read. Do not infer a role from a session title, a `/rename`, or from being in this folder.
- If the human has not named an init file, ask which file under `.grok/agents/` to read, then wait.
- Do not use a worktree. If your working directory path contains `.grok/worktrees`, stop and tell the human. Do not write.
- Grok Build Memory stays off. Do not call `/remember`. Do not treat Memory notes as binding.
- Section files under `initial_spec/sections/` are the source of truth for the workflow. Chat memory is not.
- Do not edit a product repo from this repository.
- The files under `.grok/agents/design-architect-warm.md`, `kanban-master-cold.md`, and `worker-cold.md` are templates for a product repo. Do not act as those roles here unless the human explicitly says this session is a dry run of that template.
