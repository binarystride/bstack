# Write it well

Skills aren't the only thing you ship. PR descriptions, commit messages, readmes, and the skills themselves are all prose someone has to read. This page covers cleaning that prose, holding it to a standard, and writing your own skill.

## Clean the prose with `/unslop`

```text
/unslop the readme changes, no emdashes
```

[`/unslop`](../../skills/unslop/SKILL.md) takes a target and any extra rules you have, then strips the AI tells: the throat-clearing, the false balance, the words that mean nothing. You'll develop your own shorthand. The skill reads intent fine from terse prompts like `unslop that, tighten it`.

Use it on PR descriptions and commit bodies before you post them, not after someone tells you the description reads like a press release.

## Write docs to a standard with `/technical-writing`

For docs, RFCs, readmes, PR descriptions, and commit messages:

```text
/technical-writing review the readme changes
```

[`/technical-writing`](../../skills/technical-writing/SKILL.md) applies a layered standard with one goal, prose a tired engineer understands on the first read. It picks the document's mode first (tutorial, how-to, reference, or explanation), then works sentence by sentence: who does what, one thought per sentence, nothing readable two ways. Use it to review what you or an agent just wrote, or name it up front when you ask for a doc.

## Author a focused skill

When you already know the workflow you want to capture:

```text
write a skill for verifying database migrations in this repo
```

Agent-facing prose has a higher bar than human prose, because an unhelpful sentence becomes an instruction some future agent follows. Two habits carry most of the weight:

- Start from your real history rather than from how you imagine you work. Read back over recent sessions and look for corrections you have made more than twice. Those are your rules.
- Draft it, run it on a real task, then revise. A skill written in one sitting without testing describes an aspiration, not a habit.

Run the draft through `/unslop` before you commit it, and ship it as a PR so you review it like any other change.

**Pitfall:** don't edit a skill mid-task because it's misbehaving. Fix it in its own PR and keep the task moving. A skill edit that ships tangled into feature work is invisible to review and impossible to evaluate.

Next: [Recipes and pitfalls](./07-recipes-and-pitfalls.md).
