# Git

Agents commit and push on `dev`. The human tests `dev`. When the human is happy with the current `dev`, the human merges `dev` to `main` as a release. Agents do not merge to `main`. Start from the latest `dev` before new work. No feature branch.

One change per commit. Do not mix unrelated changes.

Commit line form: `<role> <short title>`.
Not `wip`, not `step 6 done`, not `fix`.

Violation: a feature branch, a merge to `main`, or a commit that mixes two unrelated changes.
