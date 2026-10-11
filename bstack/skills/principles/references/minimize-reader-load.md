# Minimize Reader Load

Maintainability is the work a reader must do to understand code. Track two axes:

1. **Layers to trace.** How many indirections sit between the question and the answer.
2. **State to hold.** How much hidden or mutable context the reader must keep in their head.

**Why:** Code is read far more than it is written. LOC, cyclomatic complexity, and "clean architecture" are proxies. Reader load is the thing that matters. The two axes are independent. A flat file with 50 globals can be as hard to reason about as a 6-layer adapter stack. Guard both. This is the human analog of [Guard the Context Window](guard-the-context-window.md): working memory is finite for readers too.

**The pattern:**

- **Collapse layers** that do not earn their keep: pass-through wrappers, adapters with no second implementation, indirection introduced for a future that never came. A helper earns its place by hiding meaningful rules, not by having a particular number of callers.
- **Make adjacent layers change the abstraction.** A layer that repeats the same methods and arguments adds reader load without compression. Collapse pass-through layers.
- **Demand interface compression.** A broad interface that hides little complexity makes readers learn both the surface and the implementation. Prefer boundaries that hide meaningful decisions.
- **Use the deletion test.** Imagine removing the helper. If its rules would spread back across callers, keep those rules together behind a small interface. If removing it makes the whole flow easier to follow, inline or delete it.
- **Shrink state scope:** prefer pure functions (returns over mutations), locals over fields, fields over module state, and module state over globals. Derive instead of sync.
- **Name the invariant at the boundary,** not in every consumer, so the reader learns it once.
- Before adding a layer or a piece of state, ask: does this reduce reader load somewhere else by at least as much?

**The test:** Can a new reader answer "where does X come from?" and "what can change X?" in under 30 seconds? If not, cut layers or cut state.

The deletion test draws on [Matt Pocock's Codebase Design](https://github.com/mattpocock/skills/blob/d81f3a183412e71a5b1e84ca21bc1a35eea03a60/skills/engineering/codebase-design/SKILL.md). This principle is self-contained; the source skill is not required.
