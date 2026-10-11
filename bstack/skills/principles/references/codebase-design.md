# Codebase Design

Design deep modules: a lot of behavior behind a small interface, placed at a useful seam, testable through that interface. The aim is leverage for callers, locality for maintainers, and testability for everyone.

## Vocabulary

- **Module:** anything with an interface and an implementation: a function, class, package, or a slice across application layers.
- **Interface:** everything a caller must know to use the module correctly. This includes types, invariants, ordering constraints, error modes, required configuration, and performance characteristics. It is broader than a TypeScript `interface` or a function signature.
- **Implementation:** the code inside the module, including its private helpers and internal state.
- **Depth:** how much behavior a caller can exercise per unit of interface they must learn. A deep module hides substantial complexity behind a small interface. A shallow module makes callers learn almost as much as its implementation.
- **Seam:** a place where behavior can vary without editing the code that uses it. The seam's location and the behavior behind it are separate design decisions.
- **Adapter:** a concrete implementation of a dependency at a seam, such as a production transport or a test substitute.
- **Leverage:** more capability for callers with fewer rules to learn.
- **Locality:** changes, bugs, knowledge, and verification concentrate in one place. Fix once, fixed everywhere.

Use this vocabulary when it clarifies a design, while retaining the project's domain names. Do not rename established code merely to match the glossary.

## Deep versus shallow

Depth is a property of the interface, not the implementation's size. A deep module can contain small, independently testable parts. Those parts need not appear in its public interface.

When designing or changing an interface, ask:

- Can I reduce the number of operations or simplify the parameters?
- Can I hide more meaningful complexity inside?
- Which invariants, ordering rules, error handling, or configuration can callers stop managing?
- Will a future fix stay in one place, or still require coordinated edits across callers?

Do not measure depth as implementation lines divided by interface lines. That rewards padding. A long function is not automatically deep, and a short helper is not automatically shallow.

**Use the deletion test.** Imagine deleting the module. If complexity vanishes, it was a pass-through. If complexity reappears across callers, it was earning its keep. Caller count alone does not decide whether a helper is useful.

## Find deepening opportunities

Read the affected domain documentation, existing design decisions, code, callers, and tests before restructuring. Look for actual friction:

- Understanding one responsibility requires bouncing between many small files.
- Several callers repeat the same decision, state transitions, or required call sequence.
- Callers coordinate cancellation, retries, cleanup, or error handling that belong to one lifecycle.
- Tightly coupled modules expose each other's implementation details.
- An interface mostly forwards the same methods and arguments without hiding decisions.
- Extracted helpers are easy to test, but bugs still hide in how the real flow calls them.

Keep related rules together behind a small interface when this reduces what callers must know. For example, a request lifecycle can own active-request state, cancellation, completion, and cleanup while callers retain their own inputs and presentation. Returning a name for a piece of logic does not help if callers still have to orchestrate all its rules.

Apply a worthwhile design within the authorized task. Leave a wider redesign for separate work. Do not add factories, configuration, or extension points for hypothetical reuse.

## Place seams where something varies

One adapter can indicate a hypothetical seam; production and a justified test adapter can establish a real one. Prefer existing substitution points over inventing a port for every dependency. Internal test seams can stay private rather than becoming caller configuration.

| Dependency | Design and verification |
|---|---|
| In-process computation or state | Keep the behavior together and test the real module through its interface. No adapter is needed. |
| Local dependency with a faithful test substitute | Use the real dependency or that substitute in interface tests. Keep substitution internal when callers do not need to choose it. |
| Remote service owned by the project | Keep decisions in the module and use an existing transport seam, or inject a small transport interface when production and test implementations justify it. |
| Third-party service | Substitute the external boundary in tests. Keep the module's own decisions and lifecycle real. |

## Make the interface the test surface

Callers and tests should exercise the same meaningful behavior through the same interface. Verify observable results, errors, side effects, and ordering, rather than private helper calls or incidental state. If testing requires bypassing the interface, reconsider whether the module exposes the right behavior. Private computation tests can still be useful; they do not replace coverage of the complete flow.

Accept dependencies where substitution is needed rather than creating an external client inside business logic. Use existing project mechanisms. Keep calculations separate from effects under [boundary-discipline](boundary-discipline.md); a module that owns an effectful lifecycle must still be tested for those effects.

When consolidating modules, replace obsolete implementation-coupled tests with equivalent or stronger interface coverage. Establish that coverage before removing old tests, and retain distinct regression cases. Never delete or weaken a failing check to keep a refactor. Migrate internal callers together under [migrate-callers-then-delete-legacy-apis](migrate-callers-then-delete-legacy-apis.md), while preserving externally required compatibility.

## Source and adaptations

Adapted from Matt Pocock's [Codebase Design](https://github.com/mattpocock/skills/blob/d81f3a183412e71a5b1e84ca21bc1a35eea03a60/skills/engineering/codebase-design/SKILL.md) and [Deepening](https://github.com/mattpocock/skills/blob/d81f3a183412e71a5b1e84ca21bc1a35eea03a60/skills/engineering/codebase-design/DEEPENING.md). Copyright (c) 2026 Matt Pocock, MIT; the notice and permission terms are included in [LICENSE](../../../LICENSE).

BStack retains the module, interface, depth, deletion-test, seam, locality, dependency, and interface-testing guidance. It uses project vocabulary, applies scoped improvements without a mandatory interview or parallel design exercise, and replaces obsolete tests only after preserving their regression coverage. This reference is self-contained; the upstream skills are not required.
