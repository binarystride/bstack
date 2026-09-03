# Recipes and pitfalls

Prompts worth copying, then the mistakes everyone makes once. Swap in your own paths and finish conditions. The recipes are deliberately informal. That's how they get typed in practice, and the skills read intent fine.

## Understand an unfamiliar subsystem

```text
use /how first to understand how this initialization works. then use /why to figure out why it broke recently.
```

Mechanics first, history second. Each skill's report tells you which sources it searched, so you know what the answer is grounded in.

## Get a second opinion on a design

```text
/architect this. i want the caller's usage settled before any code, and two shapes compared, not one.
```

Your current design becomes one candidate among several, and the synthesis tells you whether the comparison found something better or confirmed what you had. Cheap insurance before a costly commitment.

## Review a branch skeptically

```text
/interrogate the whole branch, but skeptically. don't change anything yet. no nitpicks unless it's an actual bug or regression in behavior.
```

The qualifiers do real work. "don't change anything yet" keeps it read-only, and the nitpick rule pre-filters the noise so `Act on` findings are worth your time.

## Fix a bug through a failing test

```text
repro the duplicate write first. if there's a cheap test path, write the failing test before the fix. then fix and rerun.
```

"if there's a cheap test path" matters. Forcing a test through brittle mocks proves less than running the real command, and the agent is allowed to say so.

## Check the change before you push

```text
/review -- --full
```

No target needed. It reviews the current diff with passes that never saw your reasoning, then verifies each candidate finding before showing it to you.

## Redirect a drifting run

Steering prompts are one line:

```text
i said the goal is to repro. i did not ask for a fix yet.
```

```text
apply prove it works. show me the real output, not the build log.
```

```text
/unslop that, no emdashes
```

You rarely need more words. You need the right name, and [the principles page](./05-principles.md) is the vocabulary.

## Get the reply in plain words

```text
/bro
```

That's the whole prompt. [`/bro`](../../skills/bro/SKILL.md) restates the last message like one human talking to another, no jargon, shorter. Use it when a reply is technically thorough and you still don't know what it said.

## The pitfalls

- **Enumerating skills in the prompt.** "use /how then /architect then /interrogate" front-loads a sequence you haven't tested against the task. State the goal and constraints, then name the skill for the step you're actually on.
- **A vague finish condition.** "make it better" gives the agent nothing to check. Give a command or artifact that can pass or fail.
- **Parallel agents in one worktree.** They overwrite each other and the diff becomes archaeology. Say "own worktree per candidate" and the isolation is free.
- **Self-reviewing in the same session.** The session that wrote the code reads the diff as what it meant. `/review` runs the passes in fresh context for exactly this reason.
- **Accepting every review comment.** Bots and humans both file real catches and noise in one list. `/interrogate` sorts findings into act-on and dismissed buckets with reasons, and you can override either way.
- **Treating `auto` as a model slug.** `auto` and `inherit-parent` mean "omit the model field so the subagent inherits the parent chat model." [Setup](./01-setup.md) covers the roles.
- **Reporting success off a green build.** A build proves it compiles. Ask for the real command, flow, stored value, or profile, and expect the evidence in the reply.
- **Writing a `SKILL.md` in one sitting.** Draft it, run it on a real task, revise, then ship it as its own PR. See [Write it well](./06-writing.md).

That's the guide. If you skipped ahead, go back to [setup](./01-setup.md) and run one real task. The habits stick from use, not from reading.

Back to the [guide index](./README.md).
