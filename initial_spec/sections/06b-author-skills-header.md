## 6 continued — who may write living-knowledge, and the file header

Read after [06-knowledge.md](06-knowledge.md).

### 6.6 Skills required to *write* living-knowledge

Reading a living-knowledge file and writing one are different jobs. Workers may propose patches via ticket comments; they are not the default authors.

**Decided** by the user, including the should-haves. The author must have **software-development** skills (that is the work) and **domain knowledge of the app’s domain** so the agent is fluent in that domain’s UL.

**Must have**

1. **Software development** — enough working skill to describe real structures, not tourist architecture.
2. **Domain knowledge of this app’s subject** — fluency in the problem UL.
3. **Propositional-logic discipline** — detect contradiction; keep `A-B-x-y` and `A-B-t-w` when merging; refuse silent drop of scenario-bound detail.
4. **Uncertainty honesty (§0.5)** — write “unknown / undecided / depends on X” instead of filling holes so the file looks complete.
5. **Specification drafting** — sentences that a later agent with zero session memory can execute against.
6. **Ubiquitous Language discipline (§2.5)** — one term per concept inside a bounded context; aliases recorded; no private dialect.
7. **Software architecture (decision writing, not code copying)** — record decisions, constraints, integrations, gotchas, rejected temptations and why.
8. **Cohesion / coupling as a writing skill** — one concern per file; no spider-web of cross-references.
9. **Audience split** — overview files stay at overview altitude; detail files stay loadable 3–4 at a time without a ~30k-token prompt.
10. **Edit policy (§0.3)** — compress without deleting requirements; edit inline; no append-only diff addenda.

**Should have**

11. Requirements / problem-framing.
12. Contract / integration thinking.
13. Project/line management sense — what a later worker needs in order not to guess.
14. Information architecture of the markdown corpus itself.
15. Stack literacy for the actual stack.
16. Git skill — living-knowledge is pushed as soon as it changes.

**Must not treat as a living-knowledge skill:** javadoc-style member lists; “document every method”; unsolicited rewrite of already-stable terms; loading every implementation skill in the universe.

Writers: KanbanMaster-cold (module living-knowledge), DesignArchitect-warm (design spec + design-required living-knowledge edits), human. Both agent writers need this set.

### 6.7 Living-knowledge name, splits, and file header

**Name.** The store is **living-knowledge** (folder proposal: `/app-name-1/living-knowledge/`). Earlier drafts said “living-context.” Same artifact.

**Splits.** A file that is correct and concise can still be too large for a transient worker to load. Size is a first-class reason to split, alongside cohesion.

- Split **vertically** when two bounded contexts or two product modules do not need each other’s depth.
- Split **horizontally** when one module’s layers have different audiences (overview vs DB-detail vs UX-detail).
- Split **both** when a vertical module’s detail file alone still blows the “3–4 files ≈ 30k-token beast” budget.
- Do not split into microservice-hell (30 tiny files that all point at each other).

**Living-knowledge file header** (do not call this a “module card” — living-knowledge files are not only module files).

The header is **not** a third kind of document and **not** a replacement for the file body. It is a **fixed block at the top of each living-knowledge `.md` file**, of any type. The rest of that same file is the real content (§6.3).

The header exists so a cold worker can see, without parsing the author’s mood that day: what this file is for, what it points at, and what it refuses to answer (stop-rule).

Mandatory short header:

```text
## Living-knowledge file header
- Id: <stable-id>
- Audience: overview | detail
- Provides: <3–7 bullets a worker can trust to find in this file>
- Relies-on: <other-id> — <one line: why you would open it>
- Does-not-cover: <explicit out-of-scope>
```

No method signatures, return types, or parameter lists. If the header grows longer than useful, cut the header, not the body.
