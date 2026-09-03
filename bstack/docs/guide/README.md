# The bstack guide

bstack works best when you stop micromanaging the agent. You describe what you want and how you'll know it's done, then invoke the skill that fits the step you're on. This guide teaches that habit with realistic prompts.

Here's what you'll learn:

1. [Set up bstack](./01-setup.md). Install the plugin and pick your models.
2. [Understand the code](./02-understand.md). `/how`, `/why`, and `/teach` before you edit anything.
3. [Design the change](./03-design.md). `/architect` and `/interrogate` before code locks in a shape.
4. [Review and ship](./04-review-and-ship.md). Prove behavior on the real app, then open a focused PR.
5. [Steer with principle names](./05-principles.md). The 19 names that redirect an agent mid-task.
6. [Write it well](./06-writing.md). `/unslop`, `/technical-writing`, and authoring your own skill.
7. [Recipes and pitfalls](./07-recipes-and-pitfalls.md). Prompts to copy and mistakes to skip.

Read the pages in order the first time. After that, each page stands alone.

## If you only remember one thing

Give the agent a goal and a way to check it, in your own words:

```text
the export writes duplicate rows when a retry lands mid-run. repro first, then fix and verify.
```

"repro first" and a checkable outcome do more work than a list of skills. Name a skill when you want a specific pass: `/how` before you edit, `/review` before you push.

Next: [Set up bstack](./01-setup.md).
