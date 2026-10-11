# Code Simplification

Simplify code by reducing complexity while preserving exact behavior. The goal is not fewer lines. It is code that is easier to read, understand, modify, and debug. Ask whether a new teammate would understand the result faster than the original.

## When to simplify

Use this guidance after a feature works and relevant checks pass, during a readability refactor, or when recently changed code has accumulated duplication, nesting, unclear names, or scattered responsibilities.

Leave code alone when it is already clear, when you do not understand why it exists, or when the benefit is uncertain. Do not spend a pass simplifying code that an agreed rewrite will discard. Check performance constraints before replacing an efficient implementation with a clearer-looking but measurably slower one.

## Preserve behavior exactly

Change how the code expresses its behavior, not what it does. Before each edit, check:

- Does it produce the same output for every accepted input, including edge cases?
- Does it retain validation, error behavior, and failure recovery?
- Does it preserve side effects, their order, cancellation, cleanup, and asynchronous completion?
- Does it preserve relevant performance and resource constraints?
- Do existing behavior tests still pass without changing their expectations?

If equivalence is unclear, investigate or leave the code unchanged. Do not remove error handling because it makes the code shorter. If module restructuring requires changes to implementation-coupled tests, follow [Codebase Design's coverage rule](codebase-design.md#make-the-interface-the-test-surface).

## Understand before touching

Before changing or removing anything, understand why it exists:

- What responsibility does this code own?
- What calls it, and what does it call?
- What are the edge cases and error paths?
- Which tests and contracts define the expected behavior?
- Was the current shape chosen for performance, a platform constraint, or an earlier failure?

Read the repository's instructions and neighboring code. Check history or design records when the reason is unclear. Follow the project's naming, imports, function style, error handling, and type conventions. Renaming code to match an external preference is churn.

## Prefer clarity over cleverness

Explicit code is better than compact code when the compact version requires a mental pause. Keep direct returns when consolidating branches would introduce extra constants, flags, or mutable state that callers or readers must track. A named intermediate value earns its place only when it clarifies a real concept.

Avoid over-simplification: inlining a useful concept, combining unrelated responsibilities, or removing an abstraction that supports real testability can make the whole flow harder to understand. Use [Codebase Design](codebase-design.md) to decide whether to extract, keep, deepen, or remove a module. Judge the result across its callers, not just one function.

## Concrete opportunities

These are signals to investigate, not thresholds that require a refactor.

| Pattern | Useful response |
|---|---|
| Deep nesting or nested ternaries | Use guard clauses, explicit branches, or a named condition when they clarify the flow without changing validation or effects. |
| Long function with several responsibilities | Separate distinct responsibilities; keep a coherent lifecycle together. |
| Positional boolean flags | Use project-conventional named options or distinct operations when call sites become clearer. |
| Repeated conditions or decisions | Give the rule one owner and reuse its result. |
| Generic, abbreviated, or misleading names | Name the domain value or actual behavior, including mutation when relevant. |
| Comments explaining obvious operations | Remove them; retain comments explaining constraints and intent. |
| Duplicated related logic | Consolidate when a small interface removes rules from callers. |
| Unreachable code or unused symbols | Confirm they are dead, then remove them and obsolete imports. |
| Pass-through wrapper or speculative factory | Remove it if it hides no meaningful rule or real variation. |
| Redundant assertion or annotation | Let inference work when the compiler still proves the same contract. |
| Synchronized or overly broad state | Derive it or reduce its scope using [minimize-reader-load](minimize-reader-load.md). |

Do not automatically replace two explicit JSX branches with several conditional constants merely to share markup. Do not mechanically remove `async` or `await`, or reorder expressions: error handling, cleanup, and timing can depend on them. Understand the behavior first.

## Apply and verify incrementally

Default to the task's changed code and affected callers. Unrelated cleanup creates noisy diffs and risks regressions outside the task.

1. Choose an improvement with a concrete benefit to understanding or maintenance.
2. Make one coherent change, then run its affected checks before building more changes on it.
3. Compare the whole flow before and after. Inspect behavior, callers, project consistency, and the reviewability of the diff.
4. If the result obscures the flow or changes behavior, undo only your attempted simplification and reconsider. Never undo someone else's work or weaken checks to retain the edit.

For large repetitive refactors, prefer a checked codemod over manual edits and agree on a separate scope. Do not expand a Ship simplification pass into that work.

In Ship, retain the scoped simplification in the task's PR, in a separate commit when it changes code. A broad or independently useful refactor belongs in a separate task and PR. Ship owns pass timing, resume behavior, and review thresholds.

## Finish checks

- Existing behavior tests and the affected build, type, lint, and formatting checks pass. No assertion was relaxed to accept a behavior change.
- Inputs, outputs, errors, side effects, ordering, cleanup, and relevant performance retain their contracts.
- Each change has a readable purpose, follows project conventions, and stays within the authorized scope.
- No obsolete symbols or imports remain from the refactor.
- The whole flow is easier to understand. If it is not, revert the attempted simplification. Leaving code unchanged is a valid outcome.

## Source and adaptations

Adapted from Addy Osmani's [Code Simplification](https://github.com/addyosmani/agent-skills/blob/9d0c60d406b454a78ccc0a175b19932047aa4dac/skills/code-simplification/SKILL.md), which credits Anthropic's code-simplifier. Copyright (c) 2025 Addy Osmani, MIT; the notice and permission terms are included in [LICENSE](../../../LICENSE).

BStack retains the behavior, convention, clarity, balance, scope, understand-first, incremental-change, and verification guidance. It shares module decisions with Codebase Design, keeps scoped simplification in Ship's task PR, and treats compact-code examples as judgment calls rather than prescriptions. This reference is self-contained; the upstream skill is not required.
