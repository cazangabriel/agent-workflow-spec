## 9. Git conventions

### 9.1 Branches (decided)

One worker at a time, so **no feature branches**.

- Agents commit and push on `dev`.
- Human tests `dev`.
- When the human is happy with the current `dev`, the human merges `dev` → `main` as a release. Agents do not merge to `main`.

Start from the latest `dev` before new work.

### 9.2 Commit messages

Prefix with spawn name, ticket code, short human title: `KanbanMaster-cold F-10-DB add recipe table`.

Forbidden: commit names that only reflect internal agent steps.

### 9.3 Commit granularity

One feature per commit. Do not mix unrelated changes.

### 9.4 Persistence of living-knowledge

Living-knowledge `.md` files are committed and pushed as soon as they are modified.
