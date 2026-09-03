# Review the result and open a PR

"It compiles" is not evidence. The [Prove It Works principle](../../skills/principles/references/prove-it-works.md) makes the agent check the real artifact before it reports success, and your job is to make "the real artifact" checkable. This page covers stating a finish condition, reviewing the change with fresh eyes, and opening a PR you'd want to review.

## State the finish condition up front

Put what done means in the first prompt, in whatever words fit:

```text
add json output to this command. text output stays byte-identical, the json parses, both run against the sample project. show me the evidence.
```

Now the agent has three checks it can run, not a mood to satisfy. When the reply comes back, it should carry the exact commands and outputs. If a check couldn't run, a good reply says "inconclusive", and you should treat a confident reply without evidence as a red flag.

Match the check to the change:

- A CLI change runs the real command.
- A UI change walks the changed flow in the running app.
- A parser or migration replays a saved input.
- A perf change compares before and after profiles.
- A storage change reads back the written value.

## Give the agent a harness to drive

The UI bullet above hides a real requirement. The agent needs a scripted way to drive your app.

If your project already has one, a Playwright suite, an e2e runner, a demo recorder, an expect script, say so once and agents will reuse it. That is always the better path, because a checked-in harness knows your app's startup, env, and prompts.

**Worth building once:** a short checked-in doc naming the launch command, the env the app needs, and what a healthy start looks like. Every agent that touches the repo after you gets it for free, and it stops each one rediscovering your startup by trial and error.

## Review it before you push with `/review`

```text
/review
```

The session that wrote the code cannot review it. It still holds the plan, so it reads the diff as what it meant instead of what it says. [`/review`](../../skills/review/SKILL.md) fixes that by splitting the job across passes that never saw your reasoning: independent finder angles look for problems, and separate verifier passes decide whether each candidate is real before it reaches you.

Flags shape the pass, and they're worth knowing:

```text
/review -- --full --high
```

- `--full` runs every angle. Without it you get the three correctness angles plus conventions, and lose surfaces, cleanup, altitude, and tests.
- `--low`, `--med`, `--high` set the depth tier, which picks the model and reasoning effort every pass runs at. `--med` is the default.
- `--fix` alternates reviewing and fixing until nothing above P3 remains.
- `--base <ref>` sets what the diff is taken against.

Shorten the review by asking. The skill won't drop an angle on its own judgment, because "this looked trivial" is exactly how a one-line change to code that moves money gets missed.

## Open the PR

```text
open the pr. small ordered commits, evidence in the description.
```

Five narrow PRs beat one fat one, and stacked follow-ups beat a growing branch. Rebase the work into small ordered commits, put the evidence in the description, and run [`/unslop`](../../skills/unslop/SKILL.md) over the prose before you post it.

An open PR starts collecting blockers immediately: checks fail, reviewers comment, trunk moves. Take them in order, batch the fixes into one push so checks restart once, and stay skeptical of the review list. Humans and bots file real catches and noise together. A real finding gets a fix; noise gets dismissed with the disproof posted on the thread.

Next: [Steer with principle names](./05-principles.md).
