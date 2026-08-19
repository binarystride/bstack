# pstack

Rigorous agent workflows you can parallelize with confidence. Write less code, at higher quality.

This is a portable build of [pstack](https://github.com/cursor/plugins/tree/main/pstack) by [poteto](https://x.com/poteto). The original targets Cursor. This fork runs on any harness: it ships the Agent Plugins manifest alongside Cursor's and Claude Code's, and every skill states its intent instead of naming one vendor's models, paths, and built-in commands. See [what changed](#what-changed-from-upstream).

The point is not to maximize lines of code. It is the opposite. Go deep on one agent, trust it to write verifiable code, and you can run several of them at once without shipping slop.

## install

This directory is the plugin. The repo root above it is the marketplace.

**Codex, Cursor, GitHub Copilot, VS Code, Kiro, ChatGPT** read the [Agent Plugins](https://agent-plugins.org/) standard. Point your client at this repo and install the `pstack` plugin from the `pstack/` directory.

**Claude Code:**

```bash
/plugin marketplace add <your-org>/<this-repo>
```

```bash
/plugin install pstack@pstack
```

**Cursor** reads either manifest. Add this repo as a marketplace and install `pstack`.

Nothing to configure after install. There is no setup step and no model config file, by design: the skills describe the model *role* they need, and your harness resolves it.

## get started

Use [`/poteto-mode`](./skills/poteto-mode/SKILL.md) whenever you're doing something that needs rigor. It reads your request, picks a playbook, and runs the other skills as the steps need them.

```
/poteto-mode this pr has a subtle bug where the scroll drifts every 750ms even when idle. repro
first, then fix and verify.
```

```
/poteto-mode i'm going to bed. land the stack even if ci flakes. i want everything merged by
morning.
```

New here? The [pstack guide](./docs/guide/README.md) walks through a first real task, from prompting to verification to overnight runs.

<details>
<summary>the twenty-three playbooks</summary>

| playbook | for |
|---|---|
| [investigation](./skills/poteto-mode/playbooks/investigation.md) | a read-only question. how does x work, why was y built this way, are we sure. |
| [bug fix](./skills/poteto-mode/playbooks/bug-fix.md) | reproduce a defect, root-cause it, and fix with runtime evidence. |
| [perf](./skills/poteto-mode/playbooks/perf-issue.md) | trace a measured slowness and improve it against a baseline. |
| [hillclimb](./skills/poteto-mode/playbooks/hillclimb.md) | sustained, scientific improvement of one metric against a target, looping hypotheses with before/after measurement and one commit per accepted win. |
| [runtime forensics](./skills/poteto-mode/playbooks/runtime-forensics.md) | diagnose a live symptom (leak, idle-cpu spin, glitch) from instrumentation. |
| [trace forensics](./skills/poteto-mode/playbooks/trace-forensics.md) | diagnose a captured profiling artifact (cpuprofile, trace, spindump, heap snapshot). |
| [feature](./skills/poteto-mode/playbooks/feature.md) | new or changed behavior, built from a named data shape. |
| [refactoring](./skills/poteto-mode/playbooks/refactoring.md) | a behavior-preserving change to structure or shape. |
| [prototype](./skills/poteto-mode/playbooks/prototype.md) | a throwaway sketch to make a design or behavioral decision cheaply, or to settle an empirical fork by observing it. |
| [visual parity](./skills/poteto-mode/playbooks/visual-parity.md) | pixel-exact ui equivalence between two implementations. |
| [authoring a skill](./skills/poteto-mode/playbooks/authoring-a-skill.md) | writing or editing a SKILL.md. |
| [eval](./skills/poteto-mode/playbooks/eval.md) | test how a skill or prompt change affects agent behavior, blinded. |
| [babysit](./skills/poteto-mode/playbooks/babysit.md) | drive a pr or a stack to merge-ready: conflicts, review threads, ci. |
| [shipping](./skills/poteto-mode/playbooks/shipping.md) | independently verify a green stack, then land the contiguous verified run with graphite merge-when-ready. |
| [autonomous run](./skills/poteto-mode/playbooks/autonomous-run.md) | drive a long task to completion without stopping. |
| [orchestrate](./skills/poteto-mode/playbooks/orchestrate.md) | a standing project handed to one coordinator chat: multi-day, many stacked prs, fleets of subagents. |
| [autopilot-full](./skills/poteto-mode/playbooks/autopilot-full.md) | run independent prs to merged with one owner per pr and root verification of each merge-ready head. |
| [autopilot-stack](./skills/poteto-mode/playbooks/autopilot-stack.md) | build and verify one linear graphite stack for the operator to review and land. |
| [session pickup](./skills/poteto-mode/playbooks/session-pickup.md) | resume or take over a prior agent's in-flight work. |
| [pause safely](./skills/poteto-mode/playbooks/pause-safely.md) | suspend in-flight work cleanly so it can be resumed later. |
| [multi-phase plan](./skills/poteto-mode/playbooks/multi-phase-plan.md) | work that spans phases or stacked PRs. |
| [opening a pr](./skills/poteto-mode/playbooks/opening-a-pr.md) | the commit, description, and review-readiness pass before you open the pr. |
| [worktree cleanup](./skills/poteto-mode/playbooks/worktree-cleanup.md) | reclaim disk by pruning merged or abandoned worktrees and stale ios simulators, safety-gated. |

</details>

When invoked it:

1. Opens a todo list. The first item is reading the inline principles index in the skill.
2. Matches your task to a [playbook](./skills/poteto-mode/playbooks/) and copies the steps in verbatim.
3. Routes to the other skills as the steps fire.
4. Writes unslopped replies framed for the consumer and the maintainer.

The full rules and playbooks live in [`skills/poteto-mode/SKILL.md`](./skills/poteto-mode/SKILL.md).

It pairs well with any repeat mechanism your harness offers, so a long task can run for hours without losing rigor.

## skills

[`/poteto-mode`](./skills/poteto-mode/SKILL.md) runs most of these for you when a step needs them. The table below is for when you want one directly:

```
/how do we cancel runs? do we have an n+1 when we look up every run to cancel?
```

```
/interrogate review this pr.
```

<details>
<summary>all skills</summary>

| skill | use it when |
|---|---|
| [`/poteto-mode`](./skills/poteto-mode/SKILL.md) | default entry point for any non-trivial task. |
| [`/how`](./skills/how/SKILL.md) | you want a walkthrough of how a subsystem works. |
| [`/why`](./skills/why/SKILL.md) | you want to know why something was built this way. discovers the MCP servers you can actually reach at run time and queries each evidence category in parallel (source control, issue tracker, long-form docs, real-time chat, infra observability, error tracking, analytics warehouse). |
| [`/recall`](./skills/recall/SKILL.md) | you're starting or resuming work and want your recent context on a topic rebuilt from your own chat history and the shared record, handed back as a tight current-state brief. |
| [`/blast-radius`](./skills/blast-radius/SKILL.md) | you have a small-looking change and want to know what else it could break, with the one fact it's safe because of proven by running code, not asserted. |
| [`/architect`](./skills/architect/SKILL.md) | you're about to write code that crosses a function boundary and want the caller's usage, types, and module shape settled first. |
| [`/arena`](./skills/arena/SKILL.md) | you want N parallel attempts at the same thing, then to grab the best parts of each. |
| [`/swarm`](./skills/swarm/SKILL.md) | you want N parallel workers across different slices or races, then one aggregated report. |
| [`/interrogate`](./skills/interrogate/SKILL.md) | you have a diff and want several different models to try to break it, including a strict code-quality lens. |
| [`/reflect`](./skills/reflect/SKILL.md) | a long task landed and you want the recipe captured as a skill edit. |
| [`/teach`](./skills/teach/SKILL.md) | you want to actually understand a change or subsystem, not just have it summarized. runs how + why and weaves one plain explanation, built up diagram by diagram. |
| [`/tdd`](./skills/tdd/SKILL.md) | you're fixing a bug and there's a cheap local test path. write the failing test first, then the fix. |
| [`/no-comments`](./skills/no-comments/SKILL.md) | strip comments before review; spawns Comment Sicko, fixes accepted findings, offers encodings for claimed constraints. |
| [`/deslop`](./skills/deslop/SKILL.md) | strip AI slop from the branch diff before commit: narrating comments, defensive guards on trusted paths, `any` casts, needless nesting. |
| [`/control-cli`](./skills/control-cli/SKILL.md) | you need to drive an interactive CLI or TUI for real: deterministic input, prompt flows, startup regressions, hangs, profiles. |
| [`/control-ui`](./skills/control-ui/SKILL.md) | you need to drive a web, IDE, or Electron UI for real: screenshots, a11y snapshots, visual diffs, traces, reproducing UI bugs. |
| [`/typescript-best-practices`](./skills/typescript-best-practices/SKILL.md) | you're reading or editing typescript. grounds the type-system-discipline principle in syntax. |
| [`/figure-it-out`](./skills/figure-it-out/SKILL.md) | no bundled playbook fits. designs a rigorous, auditable playbook for the task. |
| [`/show-me-your-work`](./skills/show-me-your-work/SKILL.md) | you want a reviewable decision trail. logs decisions to a tsv you can commit. |
| [`/unslop`](./skills/unslop/SKILL.md) | you're cleaning up writing. removes AI tells. |
| [`/bro`](./skills/bro/SKILL.md) | you want the last message restated in plain human language, no jargon. |
| [`/technical-writing`](./skills/technical-writing/SKILL.md) | layered doc standard (Diátaxis + Google developer style + STE + Global English) for docs, RFCs, readmes, PR descriptions, commit messages. |

</details>

## how models are chosen

No slugs are hardcoded anywhere. Skills ask for a model by the role it plays, and you or your harness map the role to whatever you can actually reach:

- **Judgment model.** Your strongest reasoning tier. Prose, design, and the hardest changes.
- **Instruction-following model.** Your best at executing a fully specified sequence to the letter.
- **Fast code model.** Your cheapest capable tier, for mechanical edits and bulk search and read.

Where a skill wants independent reviewers or competing candidates, it asks for *different* models rather than more calls to one, preferring a different vendor over a different tier of the same family. Naming a model in your prompt overrides all of it.

## subagents

Two subagents ship in [`agents/`](./agents), which only Claude Code and Cursor load. The Agent Plugins standard leaves agents out of v1 on purpose.

- [`poteto-agent`](./agents/poteto-agent.md) is a routing target that reads `poteto-mode` in full, including its inline principles index, before doing any work. A plain general-purpose agent skips that read and drifts.
- [Comment Sicko](./agents/comment-sicko.md) is a read-only comment reviewer, usually invoked through [`/no-comments`](./skills/no-comments/SKILL.md).

On a harness with no agents directory, nothing breaks. `/no-comments` carries Comment Sicko's prompt at [`skills/no-comments/references/comment-sicko.md`](./skills/no-comments/references/comment-sicko.md) and spawns an ordinary subagent with it. `/poteto-mode` works invoked directly; you only lose the automatic routing step.

## principles

Twenty-one short skills, one principle each. `poteto-mode` indexes them inline and reads that index at task start. The standalone files are there so other skills can reference a principle by name, and so the index can point at the full rule for each.

<details>
<summary>all twenty-one principles</summary>

| principle | group | rule |
|---|---|---|
| [laziness-protocol](./skills/principle-laziness-protocol/SKILL.md) | core | Bias toward deletion and the smallest change that solves the problem. |
| [foundational-thinking](./skills/principle-foundational-thinking/SKILL.md) | core | Apply before writing logic: choosing core types and data structures, sequencing scaffold-vs-feature work, asking what concurrent actors share. Get the data structures right so downstream code becomes obvious. |
| [redesign-from-first-principles](./skills/principle-redesign-from-first-principles/SKILL.md) | core | Redesign as if the requirement had been a foundational assumption from day one, instead of bolting it on. |
| [subtract-before-you-add](./skills/principle-subtract-before-you-add/SKILL.md) | core | Remove dead weight, redundant validators, and stub references first, then build on the simpler base. |
| [minimize-reader-load](./skills/principle-minimize-reader-load/SKILL.md) | core | Count layers between question and answer, and hidden state in the reader's head; collapse one-caller wrappers and shrink mutable scope. |
| [outcome-oriented-execution](./skills/principle-outcome-oriented-execution/SKILL.md) | core | Apply during planned rewrites and migrations with explicit phase boundaries. Converge on the target architecture; don't preserve smooth intermediate states with throwaway compatibility code. |
| [experience-first](./skills/principle-experience-first/SKILL.md) | core | Choose user delight over implementation convenience; ship fewer polished features over more rough ones. |
| [exhaust-the-design-space](./skills/principle-exhaust-the-design-space/SKILL.md) | core | Build 2-3 competing prototypes and compare side by side before committing. |
| [build-the-lever](./skills/principle-build-the-lever/SKILL.md) | core | Apply to any non-trivial work, not just bulk work: edits, migrations, analyses, checks. Build the tool that does it or proves it (codemod, script, generator, or a skill your subagents follow) instead of working by hand. The tool is the artifact a reviewer can rerun. |
| [model-the-domain](./skills/principle-model-the-domain/SKILL.md) | architecture | Encode the domain in a structure instead of scattered conditionals. |
| [boundary-discipline](./skills/principle-boundary-discipline/SKILL.md) | architecture | Concentrate guards at system boundaries (CLI, config, network, external APIs); trust internal types and keep business logic in pure functions. |
| [type-system-discipline](./skills/principle-type-system-discipline/SKILL.md) | architecture | Make illegal states unrepresentable, brand semantic primitives, parse external data at boundaries, refuse to lie to the compiler, exhaust variants, derive from authoritative schemas. |
| [make-operations-idempotent](./skills/principle-make-operations-idempotent/SKILL.md) | architecture | Converge to the same end state regardless of partial prior runs. |
| [migrate-callers-then-delete-legacy-apis](./skills/principle-migrate-callers-then-delete-legacy-apis/SKILL.md) | architecture | Migrate callers and delete the old API in the same wave instead of preserving compatibility layers. |
| [separate-before-serializing-shared-state](./skills/principle-separate-before-serializing-shared-state/SKILL.md) | architecture | Eliminate the sharing first; serialize structurally only when one shared writer is a real invariant. |
| [prove-it-works](./skills/principle-prove-it-works/SKILL.md) | verification | Apply after completing a task, before declaring done. Verify against the real artifact (run the feature, read the actual value, inspect the diff), not a proxy, self-report, or 'it compiles.'. |
| [fix-root-causes](./skills/principle-fix-root-causes/SKILL.md) | verification | Trace each symptom to its root cause and fix it there; reproduce first, ask why until you reach it, resist nil-check guards that silence crashes. |
| [sequence-verifiable-units](./skills/principle-sequence-verifiable-units/SKILL.md) | verification | Apply to multi-step work (sweeps, migrations, runs of similar edits) and to how you stack commits and PRs. Break work into small units that each end in a verifiable state, check each before the next, and order delivery so the sequence proves itself to a reviewer. |
| [guard-the-context-window](./skills/principle-guard-the-context-window/SKILL.md) | delegation | Route bulk to subagents; keep summaries in the main thread, not raw payloads. |
| [never-block-on-the-human](./skills/principle-never-block-on-the-human/SKILL.md) | delegation | Proceed, present the result, let the human course-correct after the fact; reserve confirmation for irreversible actions. |
| [encode-lessons-in-structure](./skills/principle-encode-lessons-in-structure/SKILL.md) | meta | Encode the rule as a lint, metadata flag, runtime check, or script instead of more text. |

</details>

## what changed from upstream

Everything here follows upstream's intent. The changes are about making that intent survive a different harness.

- **Packaging.** Three manifests over one `skills/` tree: `plugin.json` (Agent Plugins v1), `.claude-plugin/plugin.json`, `.cursor-plugin/plugin.json`.
- **Models.** Slugs like `claude-fable-5-thinking-max` and `grok-4.6-fast-xhigh` are gone. Skills name the role and the diversity requirement instead.
- **Paths.** Nothing constructs a transcript or skills path. Harnesses disagree on both the root and the directory naming, so the skills discover it and stop if they cannot.
- **Vendor names.** No product is named as the thing to react to. PR review triage keys on "an automated reviewer", and the watcher's bot detector is a configurable list plus a shape match on the run marker in the comment body. Set `PSTACK_REVIEW_BOTS` to add yours.
- **Built-ins.** References to one harness's `/loop`, `/babysit`, and `create-skill` became descriptions of what to do, with the fallback named so nobody is stuck.
- **Vendored.** `deslop`, `control-cli`, and `control-ui` came from `cursor-team-kit`, because `poteto-mode` calls them and a plugin should not depend on a plugin you did not install. MIT, unmodified; see [LICENSE.cursor-team-kit](./LICENSE.cursor-team-kit).
- **Dropped.** `setup-pstack` (wrote a Cursor-only rules file), `automate-me`, `create-verification-skill`, `maintain-verification-skill` (all wrote into `.cursor/skills/`), and the `benny` Slack automation pack (built on Cursor Automations).
- **Sticky mode.** `poteto-mode` was a Cursor mode skill that stayed on across turns. That frontmatter is Cursor-only, so here it is an ordinary skill you invoke per task.

## license

MIT. Upstream pstack and the three vendored `cursor-team-kit` skills are MIT, Copyright (c) 2026 Cursor.
