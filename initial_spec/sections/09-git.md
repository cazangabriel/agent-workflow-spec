## 9. Git conventions

### 9.1 Gitflow stance from the brief

- Start from the latest `dev`.
- Use a feature branch when appropriate.

### 9.2 Commit messages

Every git commit for a ticket should be prefixed with the spawn name, the ticket code, and a short human title, e.g. `KanbanMaster-cold F-10-DB add recipe table`.

Purpose: the human can walk the git tree and see where work was done.

Forbidden: commit names that only reflect an agent’s internal thinking process, e.g. `apply logic GARHJE345JER3 based on internal step KSLDF9G23G48`.

### 9.3 Commit granularity

One feature per commit. Do not mix unrelated changes.

### 9.4 Persistence of living-knowledge

Living-knowledge `.md` files are committed and pushed as soon as they are modified. Context is not allowed to live only on one machine or only in one agent session.
