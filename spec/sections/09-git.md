## 9. Git conventions

### 9.1 Branches

One worker at a time, so there are no feature branches.

Required: agents commit and push on `dev`. The human tests `dev`. When the human is happy with the current `dev`, the human merges `dev` → `main` as a release. Agents do not merge to `main`. Start from the latest `dev` before new work.
Violation: an agent opening a feature branch, or an agent merging to `main`.
Example: the worker commits `Worker-cold AM-10-F-DB add recipe table` on `dev` and pushes. It does not run `git checkout -b feature/recipes`.

### 9.2 Commit messages

A commit line is: spawn name, ticket code, short human title.
Required form: `KanbanMaster-cold AM-10-F-DB add recipe table`.
Violation: a commit line that only names an internal agent step (`wip`, `step 6 done`, `fix`).
Example: `Worker-cold AM-10-F-DB add recipe table`. Not `done`.

### 9.3 Commit granularity

One feature per commit. Do not mix unrelated changes.
Violation: a commit that adds the recipe table and also restyles the login page.
Example: the recipe-table migration is its own commit. The login restyle, if it happens, is a different commit on a different ticket.

### 9.4 Persistence of living-knowledge

Living-knowledge `.md` files are committed and pushed as soon as they are modified.
Violation: a living-knowledge edit left uncommitted at the end of the step that made it.
Example: DesignArchitect-warm writes `abbreviations.md` and commits it in the same step. It does not leave the file dirty for a later session.
