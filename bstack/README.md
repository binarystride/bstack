# bstack

Rigorous agent workflows for coding agents. Fewer lines of code, at higher quality, with the evidence attached.

Each skill does one job properly: understand the code before you touch it, settle the shape before you type, review the change with passes that never saw your reasoning. Where a job is better done more than once, the skill fans out inside itself — parallel explorers, competing design candidates, independent review angles on different models — and returns one synthesized answer instead of a pile of transcripts.

It runs on any harness. Three manifests over one `skills/` tree, and every skill states the intent of what it needs instead of naming one vendor's models, paths, and built-in commands.

## install

This directory is the plugin. The repo root is the `bstack` marketplace.

**Claude Code:**

```bash
/plugin marketplace add binarystride/bstack
```

```bash
/plugin install bstack@bstack
```

**Codex:**

```bash
codex plugin marketplace add binarystride/bstack
```

```bash
codex plugin add bstack@bstack
```

**Cursor** reads either manifest. Add this repo as a marketplace and install `bstack`.

**GitHub Copilot, VS Code, Kiro, ChatGPT** read the [Agent Plugins](https://agent-plugins.org/) standard. Point your client at this repo and install the `bstack` plugin from the `bstack/` directory.

To give a whole team bstack, see [team install](../README.md#team-install).

Nothing to configure after install. There is no setup step and no model config file, by design: the skills describe the model *role* they need, and your harness resolves it.

## what it needs

Nothing, to install. Every skill is markdown and works the moment the plugin loads.

One external tool gets used, and only by the steps that reach for it:

| Tool | Needed by | Without it |
|---|---|---|
| `gh` | `/done`, when they read PRs and issues; `/ship`, to open and watch its PR | `/done` loses its PR and issue evidence; `/ship` needs another way to reach the PR |

Everything else works on plain git.

## skills

Invoke the one that matches the step you're on. Nothing routes for you, and nothing is sticky: a skill loads its rules into the conversation and they stay there until you ask for a different pass.

```text
/how do we cancel runs? do we have an n+1 when we look up every run to cancel?
```

```text
/review -- --full
```

<details>
<summary>all skills</summary>

| skill | use it when |
|---|---|
| [`/how`](./skills/how/SKILL.md) | you want a walkthrough of how a subsystem works. |
| [`/teach`](./skills/teach/SKILL.md) | you want to actually understand a change or subsystem, not just have it summarized. uses how to ground one plain explanation, built up diagram by diagram. |
| [`/architect`](./skills/architect/SKILL.md) | you're about to write code that crosses a function boundary and want the caller's usage, types, and module shape settled first. |
| [`/review`](./skills/review/SKILL.md) | you're about to push and want the change reviewed by passes that never saw your reasoning, each candidate finding verified before it reaches you. |
| [`/interrogate`](./skills/interrogate/SKILL.md) | you have a diff and want several different models to try to break it, including a strict code-quality lens. |
| [`/walk-bug`](./skills/walk-bug/SKILL.md) | you have one bug report, error, or complaint and want the cause found and explained in the shape of a good PR description: one-sentence verdict, things to note, the failure path drawn as call trees, pseudocode, or diffs, then the fix. |
| [`/walk-me`](./skills/walk-me/SKILL.md) | you have a list of findings and want them one at a time: short explanation, recommended fix, your decision, then the change. |
| [`/ship`](./skills/ship/SKILL.md) | you want one ticket taken to a merge-ready PR while you're away: evidence first, the smallest design, self-review, then a loop with your review bot capped at five runs. add `--fast` to defer P2/P3 findings while retaining both reviews and blocking on P0/P1. ends ready, needs decision, or stopped, and never merges. |
| [`/typescript-best-practices`](./skills/typescript-best-practices/SKILL.md) | you're reading or editing typescript. grounds the type-system-discipline principle in syntax. |
| [`/unslop`](./skills/unslop/SKILL.md) | you explicitly ask to remove AI tells from writing. opt-in only. |
| [`/technical-writing`](./skills/technical-writing/SKILL.md) | layered doc standard (Diátaxis + Google developer style + STE + Global English) for docs, RFCs, readmes, PR descriptions, commit messages. |
| [`/done`](./skills/done/SKILL.md) | you're about to archive a thread and want to know what's still open. checks the durable record, then reports a short done summary plus open items and who owns them. |
| [`/bro`](./skills/bro/SKILL.md) | you want the last message restated in plain human language, no jargon. |

</details>

New here? The [bstack guide](./docs/guide/README.md) walks through a first real task, from understanding the code to reviewing the change.

## how models are chosen

No slugs are hardcoded anywhere. Skills ask for a model by the role it plays, and you or your harness map the role to whatever you can actually reach:

- **Judgment model.** Your strongest reasoning tier. Prose, design, and the hardest changes.
- **Instruction-following model.** Your best at executing a fully specified sequence to the letter.
- **Fast code model.** Your cheapest capable tier, for mechanical edits and bulk search and read.

Where a skill wants independent reviewers or competing candidates, it asks for *different* models rather than more calls to one, preferring a different vendor over a different tier of the same family. Naming a model in your prompt overrides all of it.

## subagents

Three subagents ship in [`agents/`](./agents), which only Claude Code loads. The Agent Plugins standard leaves agents out of v1 on purpose.

- [`review-angle-low`](./agents/review-angle-low.md), [`review-angle-med`](./agents/review-angle-med.md), and [`review-angle-high`](./agents/review-angle-high.md) are the passes [`/review`](./skills/review/SKILL.md) spawns. One per depth tier, because Claude Code sets reasoning effort in an agent definition rather than per spawn.

On a harness with no agents directory, nothing breaks. `/review` runs its passes on whatever the harness gives it and says so in the report.

## principles

Twenty short rules, one per file, indexed by the [`principles`](./skills/principles/SKILL.md) skill. They are reference files rather than skills of their own, so they don't crowd your slash menu. Other skills cite a principle by name and link the rule. You steer with the names too: one phrase redirects the work more precisely than a paragraph of instructions.

<details>
<summary>all twenty principles</summary>

| principle | group | rule |
|---|---|---|
| [laziness-protocol](./skills/principles/references/laziness-protocol.md) | core | Bias toward deletion and the smallest change that solves the problem. |
| [foundational-thinking](./skills/principles/references/foundational-thinking.md) | core | Apply before writing logic: choosing core types and data structures, sequencing scaffold-vs-feature work, asking what concurrent actors share. Get the data structures right so downstream code becomes obvious. |
| [redesign-from-first-principles](./skills/principles/references/redesign-from-first-principles.md) | core | Redesign as if the requirement had been a foundational assumption from day one, instead of bolting it on. |
| [subtract-before-you-add](./skills/principles/references/subtract-before-you-add.md) | core | Remove dead weight, redundant validators, and stub references first, then build on the simpler base. |
| [minimize-reader-load](./skills/principles/references/minimize-reader-load.md) | core | Count layers between question and answer, and hidden state in the reader's head; collapse one-caller wrappers and shrink mutable scope. |
| [outcome-oriented-execution](./skills/principles/references/outcome-oriented-execution.md) | core | Apply during planned rewrites and migrations with explicit phase boundaries. Converge on the target architecture; don't preserve smooth intermediate states with throwaway compatibility code. |
| [experience-first](./skills/principles/references/experience-first.md) | core | Choose user delight over implementation convenience; ship fewer polished features over more rough ones. |
| [exhaust-the-design-space](./skills/principles/references/exhaust-the-design-space.md) | core | Build 2-3 competing prototypes and compare side by side before committing. |
| [model-the-domain](./skills/principles/references/model-the-domain.md) | architecture | Encode the domain in a structure instead of scattered conditionals. |
| [boundary-discipline](./skills/principles/references/boundary-discipline.md) | architecture | Concentrate guards at system boundaries (CLI, config, network, external APIs); trust internal types and keep business logic in pure functions. |
| [type-system-discipline](./skills/principles/references/type-system-discipline.md) | architecture | Make illegal states unrepresentable, brand semantic primitives, parse external data at boundaries, refuse to lie to the compiler, exhaust variants, derive from authoritative schemas. |
| [make-operations-idempotent](./skills/principles/references/make-operations-idempotent.md) | architecture | Converge to the same end state regardless of partial prior runs. |
| [migrate-callers-then-delete-legacy-apis](./skills/principles/references/migrate-callers-then-delete-legacy-apis.md) | architecture | Migrate callers and delete the old API in the same wave instead of preserving compatibility layers. |
| [separate-before-serializing-shared-state](./skills/principles/references/separate-before-serializing-shared-state.md) | architecture | Eliminate the sharing first; serialize structurally only when one shared writer is a real invariant. |
| [prove-it-works](./skills/principles/references/prove-it-works.md) | verification | Apply after completing a task, before declaring done. Verify against the real artifact (run the feature, read the actual value, inspect the diff), not a proxy, self-report, or 'it compiles.'. |
| [evidence-before-code](./skills/principles/references/evidence-before-code.md) | verification | Write code only for observed cases: every branch, guard and fix traces to a production count, a log line, a document, a spec rule, or the requirement. Label each claim confirmed, inferred, or unverified. |
| [fix-root-causes](./skills/principles/references/fix-root-causes.md) | verification | Trace each symptom to its root cause and fix it there; reproduce first, ask why until you reach it, resist nil-check guards that silence crashes. |
| [guard-the-context-window](./skills/principles/references/guard-the-context-window.md) | delegation | Route bulk to subagents; keep summaries in the main thread, not raw payloads. |
| [never-block-on-the-human](./skills/principles/references/never-block-on-the-human.md) | delegation | Proceed, present the result, let the human course-correct after the fact; reserve confirmation for irreversible actions. |
| [encode-lessons-in-structure](./skills/principles/references/encode-lessons-in-structure.md) | meta | Encode the rule as a lint, metadata flag, runtime check, or script instead of more text. |

</details>

## license

MIT. bstack started as a portable build of [poteto's pstack](https://github.com/cursor/plugins/tree/main/pstack), which is MIT licensed, and much of its text still comes from pstack. Both copyright notices are in [`LICENSE`](./LICENSE).
