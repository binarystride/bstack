# Make it yours

mode is one person's style. The machinery underneath, playbooks, routing, model roles, works just as well wearing yours. This page covers generating a personal mode, capturing lessons from a session, authoring a focused skill, and testing a skill change before you trust it.

## Write your own mode skill

`mode` is a skill like any other. To get your own, copy its shape and change the parts that are opinions rather than machinery.

What is machinery, and worth keeping:

- The todo list whose first item is reading the principles index.
- The trigger list that maps a request to a playbook.
- The model roles, so delegation is deliberate rather than accidental.
- The routing into `how`, `why`, `architect`, `interrogate`, `unslop`, and the rest.

What is opinion, and yours to change: the reply shape, how much it asks before acting, which playbooks you actually use, and how hard it pushes on verification.

Start from your real history rather than from how you imagine you work. Read back over your recent sessions and look for corrections you have made more than twice; those are your rules. Then draft the skill, run it on a real task, and revise. A mode skill written in one sitting without testing describes an aspiration, not a habit.

Run the draft through [`/unslop`](../../skills/unslop/SKILL.md) before you commit it, and ship it as a PR so you review it like any other change.

## Author a focused skill

When you already know the workflow you want to capture:

```text
/mode write a skill for verifying database migrations in this repo
```

Writing a skill matches the [Authoring or modifying a skill playbook](../../skills/mode/playbooks/authoring-a-skill.md), which routes through your harness's skill-authoring flow when it has one, validates the frontmatter and links, and ships the result through the Opening a PR playbook. Agent-facing prose has a higher bar than human prose, because an unhelpful sentence becomes an instruction some future agent follows. Let the playbook hold that bar rather than writing a `SKILL.md` freehand.

One special case is worth splitting out. A skill that drives your app and proves behavior should wrap the repo's own harness rather than restate it, so read [Verify and ship](./06-verify-and-ship.md#give-the-agent-a-harness-to-drive) first and build on [`/control-cli`](../../skills/control-cli/SKILL.md) or [`/control-ui`](../../skills/control-ui/SKILL.md).

## Write docs to a standard with `/technical-writing`

Skills aren't the only prose you ship. For docs, RFCs, readmes, PR descriptions, and commit messages:

```text
/technical-writing review the readme changes
```

[`/technical-writing`](../../skills/technical-writing/SKILL.md) applies a layered standard with one goal, prose a tired engineer understands on the first read. It picks the document's mode first (tutorial, how-to, reference, or explanation), then works sentence by sentence: who does what, one thought per sentence, nothing readable two ways. Use it to review what you or an agent just wrote, or name it up front when you ask for a doc.

## Test a skill change blind

A skill edit affects every future session, so test it like the experiment it is:

```text
/mode run the eval playbook on this skill change. same task for both variants, candidates stay blind.
```

The [Eval playbook](../../skills/mode/playbooks/eval.md) is built around one failure mode, the observer effect. An agent that knows it's being evaluated behaves differently. So candidate agents get an organic-looking task in sanitized directories, never the words "eval" or "candidate", and never each other's existence. One judge scores all outputs under neutral labels, and chain-following gets graded from which files each candidate actually read, not from what it claims.

Read every output yourself before accepting the verdict. If you disagree with the judge, suspect the rubric before you suspect your judgment.

**Pitfall:** don't edit a skill mid-task because it's misbehaving. Fix it in its own PR and keep the task moving. A skill edit that ships tangled into feature work is invisible to review and impossible to evaluate.

Next: [Recipes and pitfalls](./10-recipes-and-pitfalls.md).
