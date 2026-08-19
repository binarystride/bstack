# Verify the result and open a PR

"It compiles" is not evidence. The [Prove It Works principle](../../skills/principle-prove-it-works/SKILL.md) makes the agent check the real artifact before it reports success, and your job is to make "the real artifact" checkable. This page covers stating a finish condition, generating a verification skill for your app, opening the PR, and driving it to merged.

## State the finish condition up front

Put what done means in the first prompt, in whatever words fit:

```text
/poteto-mode add json output to this command. text output stays byte-identical, the json parses, both run against the sample project. show me the evidence.
```

Now the agent has three checks it can run, not a mood to satisfy. When the reply comes back, it should carry the exact commands and outputs. If a check couldn't run, a good reply says "inconclusive", and you should treat a confident reply without evidence as a red flag.

Match the check to the change:

- A CLI change runs the real command.
- A UI change walks the changed flow in the running app.
- A parser or migration replays a saved input.
- A perf change compares before and after profiles.
- A storage change reads back the written value.

For a small diff you don't fully trust, [`/blast-radius`](../../skills/blast-radius/SKILL.md) finds what it could break elsewhere. It picks the one fact the change is safe because of and proves it by running code instead of writing an essay about it.

## Give the agent a harness to drive

The UI bullet above hides a real requirement. The agent needs a scripted way to drive your app.

If your project already has one, a Playwright suite, an e2e runner, a demo recorder, an expect script, say so once and agents will reuse it. That is always the better path, because a checked-in harness knows your app's startup, env, and prompts.

If not, two skills build a temporary one from standard local tools:

```text
/control-cli reproduce the prompt flow that hangs on an empty config
```

```text
/control-ui screenshot the settings panel before and after, at 1280x800
```

[`/control-cli`](../../skills/control-cli/SKILL.md) drives interactive CLIs and TUIs: tmux `send-keys` and `capture-pane`, a PTY probe where tmux is unavailable, and the Node or Bun inspector for CPU profiles, heap snapshots, and hangs. [`/control-ui`](../../skills/control-ui/SKILL.md) drives web, IDE, and Electron UIs: Playwright against the dev server, `connectOverCDP` for Chromium apps, screenshots, accessibility snapshots, traces, and visual diffs.

Both prefer accessibility roles and stable `data-*` selectors over coordinates, and neither adds a dependency to your project just to run a probe.

Once driving the app is repeatable, "verify it in the app" becomes a step any agent can execute here with no setup conversation, and a [`/swarm`](../../skills/swarm/SKILL.md) can split a full pass by feature and aggregate the results.

**Worth building once:** a short checked-in doc naming the launch command, the env the app needs, and what a healthy start looks like. Every agent that touches the repo after you gets it for free, and it stops each one rediscovering your startup by trial and error.

## Open the PR

```text
/poteto-mode open the pr. small ordered commits, evidence in the description.
```

The [Opening a PR playbook](../../skills/poteto-mode/playbooks/opening-a-pr.md) works from a worktree, rebases the work into small ordered commits, cleans the diff, unslops the prose, and returns the PR link. Five narrow PRs beat one fat one, and stacked follow-ups beat a growing branch.

## Drive the PR to merge-ready with Babysit

An open PR starts collecting blockers immediately. Checks fail, reviewers comment, trunk moves. Hand that churn to the [Babysit playbook](../../skills/poteto-mode/playbooks/babysit.md):

```text
/poteto-mode babysit this pr. get it green.
```

Babysit watches the PR with a bundled watcher and takes blockers in order: conflicts, then review threads, then CI. Every known fix batches into one push, so the checks restart once instead of after every fix. The comment triage is skeptical, because humans and bots file real catches and noise in the same list. A real finding gets a fix, and noise gets dismissed with the disproof posted on the thread. When all you want is status, ask smaller and Babysit answers without starting the loop:

```text
/poteto-mode check on pr 123. anything outstanding?
```

Babysit stops at merge-ready. It never merges, even with everything green, because merging is a different decision.

## Land the stack with Shipping

Green is not the same as safe. When you're ready to land, say so:

```text
/poteto-mode land the stack.
```

The [Shipping playbook](../../skills/poteto-mode/playbooks/shipping.md) verifies each PR independently before it merges anything. One fresh agent per PR proves the behavior live, and the agent that judges a change is never the one that wrote it. Then Shipping lands only the contiguous verified run from the bottom, one PR per cycle, and reports the first PR that breaks the chain. A verified PR sitting above an unverified one waits, because merging it would pull the gap in underneath.

Next: [Run work while you sleep](./07-overnight.md).
