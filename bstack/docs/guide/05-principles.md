# Steer with principle names

bstack ships 20 principles, one rule per file under the [`principles`](../../skills/principles/SKILL.md) skill. Other skills cite them by name, and an agent that applies one should name it in its reply along with the decision it changed.

You don't invoke principles. You use their names to steer. Each name points at a complete rule the agent has already read, so one phrase redirects the work more precisely than a paragraph of instructions.

## Steering in practice

Say the agent is about to bolt a new adapter onto three existing ones:

```text
use subtract before you add. delete the obsolete adapters first, then design what's left.
```

Say it claims success because the build passed:

```text
apply prove it works. run the real import flow and show me the written records.
```

Say two parallel attempts are about to write to the same branch:

```text
separate before serializing shared state. give each attempt its own worktree, no locks.
```

Each phrase lands because the rule behind it is specific. The agent still has to say, in its reply, which decision the rule changed. A principle citation with no decision behind it is the tell that it name-dropped instead of applying.

## The 20, briefly

The core principles decide how much to build and when to rethink the design:

- [Laziness Protocol](../../skills/principles/references/laziness-protocol.md) prefers deletion and the smallest change that solves the problem.
- [Foundational Thinking](../../skills/principles/references/foundational-thinking.md) chooses the core data structures before writing logic.
- [Redesign from First Principles](../../skills/principles/references/redesign-from-first-principles.md) integrates a new requirement as if it had been there from day one.
- [Subtract Before You Add](../../skills/principles/references/subtract-before-you-add.md) removes dead weight before building on top of it.
- [Minimize Reader Load](../../skills/principles/references/minimize-reader-load.md) collapses layers and hidden state a reader must hold in their head.
- [Outcome-Oriented Execution](../../skills/principles/references/outcome-oriented-execution.md) converges rewrites on the target design instead of preserving throwaway compatibility states.
- [Experience First](../../skills/principles/references/experience-first.md) chooses the user's result over implementation convenience.
- [Exhaust the Design Space](../../skills/principles/references/exhaust-the-design-space.md) builds two or three competing prototypes when there's no precedent.

The architecture principles decide where state, validation, and compatibility live:

- [Model the Domain](../../skills/principles/references/model-the-domain.md) encodes repeated rules in one structure, not scattered conditionals.
- [Boundary Discipline](../../skills/principles/references/boundary-discipline.md) validates at the boundary and trusts internal types.
- [Type System Discipline](../../skills/principles/references/type-system-discipline.md) makes illegal states unrepresentable.
- [Make Operations Idempotent](../../skills/principles/references/make-operations-idempotent.md) converges retries on the same end state.
- [Migrate Callers Then Delete Legacy APIs](../../skills/principles/references/migrate-callers-then-delete-legacy-apis.md) migrates and deletes in one wave.
- [Separate Before Serializing Shared State](../../skills/principles/references/separate-before-serializing-shared-state.md) removes the sharing before adding coordination.

The verification principles define what counts as proof:

- [Prove It Works](../../skills/principles/references/prove-it-works.md) verifies the real artifact, not a proxy.
- [Evidence Before Code](../../skills/principles/references/evidence-before-code.md) writes code only for cases someone has observed, and labels every claim.
- [Fix Root Causes](../../skills/principles/references/fix-root-causes.md) reproduces and traces to the cause before changing code.

The delegation principles keep parallel work sane:

- [Guard the Context Window](../../skills/principles/references/guard-the-context-window.md) routes bulk reading to subagents and keeps findings in the main chat.
- [Never Block on the Human](../../skills/principles/references/never-block-on-the-human.md) proceeds on reversible work and presents the result.

And one meta principle:

- [Encode Lessons in Structure](../../skills/principles/references/encode-lessons-in-structure.md) turns advice you've repeated twice into a lint, check, or script.

Don't memorize the list. Skim it now, then come back when you catch the agent doing something a name here would have prevented. That's how the vocabulary sticks.

Next: [Write it well](./06-writing.md).
