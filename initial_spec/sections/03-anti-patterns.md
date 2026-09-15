## 3. Scenarios to avoid (anti-patterns)

### 3.1 The expensive god-agent

Do not have one god agent that knows every project detail, with a huge and growing context that is extremely expensive to prompt.

Concrete failure from the user’s history: an agent that consumed about 100k tokens per prompt. That burns through Super Grok subscription credits quickly under Grok Build.

This is given as reductio ad absurdum. The usable rule: do not use expensive agents with massive context as the default working unit.

### 3.2 Too many specialists and a dense communication graph

Many specialised agents create many communication channels. That communication costs tokens for input, output, and logs.

Worst case: the communication graph among agents resembles a connected graph (every agent talks to every other). Avoid that.

Mitigations stated in the brief:

- Keep the number of agents limited so channel count stays organically small.
- Agents must not be “super chatty.” Stay on project / feature / workflow details. Not dinner conversation.
- Avoid endless discussion threads. If a thread tends to become long, an agent must be designated to escalate that thread to the user so the user decides what to do.
- Watch for loops in inter-agent discussion and break them by escalation rather than more talk.
