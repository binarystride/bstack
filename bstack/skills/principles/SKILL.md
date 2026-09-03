---
name: principles
description: "The nineteen bstack principles, one rule each. Read when a principle is cited by name, in a skill, a review, or the user's own words, and you need the rule behind the name rather than the label."
---

# Principles

One rule per file under [`references/`](references/). A skill or a person cites a principle by name; this index says what each name means and points at the full rule.

Cite a principle only with the decision it changed. A citation with no decision behind it is the tell that the name was dropped rather than applied.

## Core

How much to build, and when to rethink the design.

| principle | rule |
|---|---|
| [laziness-protocol](references/laziness-protocol.md) | Bias toward deletion and the smallest change that solves the problem. |
| [foundational-thinking](references/foundational-thinking.md) | Apply before writing logic: choosing core types and data structures, sequencing scaffold-vs-feature work, asking what concurrent actors share. Get the data structures right so downstream code becomes obvious. |
| [redesign-from-first-principles](references/redesign-from-first-principles.md) | Redesign as if the requirement had been a foundational assumption from day one, instead of bolting it on. |
| [subtract-before-you-add](references/subtract-before-you-add.md) | Remove dead weight, redundant validators, and stub references first, then build on the simpler base. |
| [minimize-reader-load](references/minimize-reader-load.md) | Count layers between question and answer, and hidden state in the reader's head; collapse one-caller wrappers and shrink mutable scope. |
| [outcome-oriented-execution](references/outcome-oriented-execution.md) | Apply during planned rewrites and migrations with explicit phase boundaries. Converge on the target architecture; don't preserve smooth intermediate states with throwaway compatibility code. |
| [experience-first](references/experience-first.md) | Choose user delight over implementation convenience; ship fewer polished features over more rough ones. |
| [exhaust-the-design-space](references/exhaust-the-design-space.md) | Build 2-3 competing prototypes and compare side by side before committing. |

## Architecture

Where state, validation, and compatibility live.

| principle | rule |
|---|---|
| [model-the-domain](references/model-the-domain.md) | Encode the domain in a structure instead of scattered conditionals. |
| [boundary-discipline](references/boundary-discipline.md) | Concentrate guards at system boundaries (CLI, config, network, external APIs); trust internal types and keep business logic in pure functions. |
| [type-system-discipline](references/type-system-discipline.md) | Make illegal states unrepresentable, brand semantic primitives, parse external data at boundaries, refuse to lie to the compiler, exhaust variants, derive from authoritative schemas. |
| [make-operations-idempotent](references/make-operations-idempotent.md) | Converge to the same end state regardless of partial prior runs. |
| [migrate-callers-then-delete-legacy-apis](references/migrate-callers-then-delete-legacy-apis.md) | Migrate callers and delete the old API in the same wave instead of preserving compatibility layers. |
| [separate-before-serializing-shared-state](references/separate-before-serializing-shared-state.md) | Eliminate the sharing first; serialize structurally only when one shared writer is a real invariant. |

## Verification

What counts as proof.

| principle | rule |
|---|---|
| [prove-it-works](references/prove-it-works.md) | Apply after completing a task, before declaring done. Verify against the real artifact (run the feature, read the actual value, inspect the diff), not a proxy, self-report, or 'it compiles.'. |
| [fix-root-causes](references/fix-root-causes.md) | Trace each symptom to its root cause and fix it there; reproduce first, ask why until you reach it, resist nil-check guards that silence crashes. |

## Delegation

Keeping parallel work sane.

| principle | rule |
|---|---|
| [guard-the-context-window](references/guard-the-context-window.md) | Route bulk to subagents; keep summaries in the main thread, not raw payloads. |
| [never-block-on-the-human](references/never-block-on-the-human.md) | Proceed, present the result, let the human course-correct after the fact; reserve confirmation for irreversible actions. |

## Meta

Turning a repeated correction into structure.

| principle | rule |
|---|---|
| [encode-lessons-in-structure](references/encode-lessons-in-structure.md) | Encode the rule as a lint, metadata flag, runtime check, or script instead of more text. |
