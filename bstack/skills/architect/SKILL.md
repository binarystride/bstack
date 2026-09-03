---
name: architect
description: "Sketch types, signatures, and module structure before code, then stay in the loop while implementation fills in. Use for /architect, 'architect this', 'design this', or non-trivial work where jumping to code would lock in the wrong shape."
disable-model-invocation: true
---

# Architect

Design before implementing. Sketch types, function signatures, class shapes, and module boundaries with `not implemented` bodies and pseudocode. Synthesize across multiple model perspectives, then fill in code against the chosen sketch. If implementation proves the sketch wrong, throw it out and redesign.

## Start

Open a todolist with one entry per phase before starting. Running without a checkpoint needs the list to show phase position and keep phases from silently disappearing.

1. Ground
2. Sketch
3. Agree
4. Implement
5. Scrap

## Phase A: Ground the problem

Build a real mental model of every system the new code touches. Run the **how** skill over the relevant subsystems. Critique mode if existing structure is the constraint or the design must push back on it.

Naming a file isn't grounding. Produce the traced model `how` prescribes. If the design redefines ownership or layering, also run the **why** skill on the existing shape so the rationale becomes a constraint, not a guess.

Skip Phase A only when the work is genuinely greenfield with no surrounding system to integrate.

## Phase B: Sketch

Design it twice. Require at least two structurally distinct candidates before synthesis, even when the first looks sufficient. This is the [**exhaust-the-design-space**](../principles/references/exhaust-the-design-space.md) principle made concrete. Whole-shape alternatives, not point fixes inside one shape.

Each candidate follows [`references/runner-prompt.md`](references/runner-prompt.md) and produces a design package shaped per [`references/rationale-template.md`](references/rationale-template.md): the caller's usage written first, then the type sketch, function signatures, module map, and prose rationale derived from it.

Where your harness has subagents, run one candidate per distinct model you can reach, up to four, preferring different vendors, each writing to its own directory. Design divergence is what you are buying. Without subagents, sketch the candidates yourself, one at a time, and write each one out in full before starting the next. A candidate you only imagined is a candidate you cannot compare.

Screen every candidate against [`references/design-red-flags.md`](references/design-red-flags.md) before synthesis. Reject or revise shallow modules, information leakage, temporal decomposition, and pass-through methods.

Compare viable candidates on interface depth. Prefer the design that hides more complexity behind a smaller, simpler public surface. A rich interface can keep call chains short by concentrating capability instead of scattering it across layers.

Synthesize one design package. Pick as the base the candidate a future maintainer can extend most easily without breaking invariants, then fold in by hand what is worth keeping from the others; don't paste mechanically, the result has to stay coherent under one mental model. Record the base, the grafts, and what you rejected and why in the rationale's "Synthesis decision" section. When the candidates converge on the same shape, note the convergence and ship it; no graft is needed.

## Phase C: Agree (opt-in)

Default: proceed directly to implementation with the synthesized design. No human checkpoint, per the [**never-block-on-the-human**](../principles/references/never-block-on-the-human.md) principle. The design is reversible and reviewable, so the human course-corrects on the diff rather than on a prompt.

Opt in to a checkpoint when the invoker explicitly asks: "/architect with checkpoint," "stop and show me before implementing," or similar. Then surface the synthesized design and pause for sign-off.

The synthesis can ship as its own commit either way. That's the "scaffold first" mode of the [**foundational-thinking**](../principles/references/foundational-thinking.md) principle; subsequent commits read as filling in bodies against a stable contract. Planned and scoped breakage during fill-in is fine, per the [**outcome-oriented-execution**](../principles/references/outcome-oriented-execution.md) principle. For adversarial pressure on the design before implementing, run the **interrogate** skill on the synthesized sketch.

If the human pushes back on the shape (in a checkpoint or after the fact), treat that as Phase A evidence. Re-ground and re-run Phase B before writing more code.

## Phase D: Implement against the sketch

Replace `not implemented` bodies with code, pseudocode with logic. The synthesized sketch is the contract.

Deviations from the sketch are signal worth surfacing, not friction to absorb silently. If a function needs a parameter the sketch didn't anticipate, ask whether the sketch was wrong, the requirement was missed, or the implementation is overreaching. Surface it; don't bolt it on.

When the sketch replaces an existing API, migrate its callers and delete the old one in the same wave, per the [**migrate-callers-then-delete-legacy-apis**](../principles/references/migrate-callers-then-delete-legacy-apis.md) principle. A compatibility layer kept alive only for internal callers is dual-path complexity the design did not ask for.

A synthesized design earns no pass. Verify the filled-in code against the real artifact, per the [**prove-it-works**](../principles/references/prove-it-works.md) principle, and treat a delegate's summary of its own work as a claim rather than evidence.

## Phase E: Scrap when the architecture is wrong

If implementation keeps producing friction the sketch can't absorb, throw the sketch out. Don't bolt fixes onto a wrong design, per the **redesign-from-first-principles** and [**fix-root-causes**](../principles/references/fix-root-causes.md) principles.

The signal is a *pattern*, not single instances. Tells:

- The same shape of workaround appearing repeatedly across unrelated code.
- Multiple unrelated edge cases that all need special-case branches.
- Types that need escape hatches (`any`, casts, optional fields always set in practice) to compile.
- The "we need a lock" reflex when the sketch said the state wasn't shared.
- Callers having to know the abstraction's internal rules to use it.
- Two or more independent Phase D deviations of the same shape across the implementation. Surfacing deviations is Phase D's job; a repeated pattern of them is Phase E's trigger.

Use judgment. A few edge cases don't condemn an architecture. Some problems are legitimately complex; complexity in the data is not complexity in the design. The rewrite signal is repeated friction of the same shape, not single hard cases.

When you scrap:

1. Re-run the **how** skill over what's been built. The implementation lessons enter the new design as inputs, not vibes.
2. Redesign as if the new constraints had been day-one assumptions, per redesign-from-first-principles.
3. Subtract before adding, per the [**subtract-before-you-add**](../principles/references/subtract-before-you-add.md) principle. The new sketch should be smaller than the old one before it grows.
4. Return to Phase B and sketch again.

## Outputs

The caller's usage is written first and the type sketch derived from it. One file with new types and signatures for small changes; module map plus type definitions for larger work. The rationale ships alongside, shaped per `references/rationale-template.md`, including the usage sketch and the synthesis decision.
