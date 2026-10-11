# Minimize Reader Load

Maintainability is the work a reader must do to understand code. Track two axes:

1. **Layers to trace.** How many indirections sit between the question and the answer.
2. **State to hold.** How much hidden or mutable context the reader must keep in their head.

**Why:** Code is read far more than it is written. LOC, cyclomatic complexity, and "clean architecture" are proxies. Reader load is the thing that matters. The two axes are independent. A flat file with 50 globals can be as hard to reason about as a 6-layer adapter stack. Guard both. This is the human analog of [Guard the Context Window](guard-the-context-window.md): working memory is finite for readers too.

**The pattern:**

- **Assess layers with [Codebase Design](codebase-design.md).** It owns interface depth, the deletion test, locality, seam placement, and interface tests. Count what callers must understand, not just files or function calls.
- **Shrink state scope:** prefer locals over fields, fields over module state, and module state over globals. Derive values instead of maintaining synchronized copies.
- **Name the invariant at the boundary,** not in every consumer, so the reader learns it once.
- Before adding a layer or a piece of state, ask: does this reduce reader load somewhere else by at least as much?

**The test:** Can a new reader answer "where does X come from?" and "what can change X?" in under 30 seconds? If not, find the layer or state ownership that hides the answer and simplify it using [Code Simplification](code-simplification.md).
