# Set up bstack

In this page you install the plugin and run your first task. There is no configuration step.

## Install the plugin

See the [install section of the readme](../../README.md#install) for the command your harness uses. Every one of them installs the same directory.

## There is nothing to configure

The plugin holds no state and reads no config file. Install it and it works. There is no setup command, no model mapping to write, and no environment variable to set.

bstack never names a model. Skills ask for a model by the role it plays, and the agent maps the role to what it can actually reach in your session:

- **Judgment model.** Your strongest reasoning tier. Prose, design, and the hardest changes.
- **Instruction-following model.** Your best at executing a fully specified sequence to the letter.
- **Fast code model.** Your cheapest capable tier, for mechanical edits and bulk search and read.

Where a step wants independent reviewers or competing candidates, it asks for *different* models rather than more calls to one, preferring a different vendor over a different tier of the same family. If you can only reach one model, the step still runs and says so.

To pin a role, say so in the prompt ("use opus for the judge"), or write one line in whatever personal instructions file your harness already reads. A prompt beats every default in the skills.

## Give the agent a way to drive your app

Most of bstack's verification steps assume the agent can exercise the real app. If your repo has a test or demo harness, agents will find and reuse it. If not, expect the agent to build a throwaway one from standard local tools, and expect that to be slower and less reliable than the harness you could check in.

A repo with a checked-in harness gets better results than one without, because the agent stops guessing how to launch and drive the thing. That is worth building once.

## Run your first task

Pick something real but small, and start by understanding the code you're about to touch:

```text
/how does this command build its output? i want to add a --json flag without changing the text form.
```

Then state the change and the finish condition in one prompt:

```text
add a --json flag to this command. text output stays byte-identical. verify both.
```

Invoking a skill loads its rules into the conversation, and they stay there, so follow-ups keep applying them. You only re-invoke when you want a different pass.

Next: [Understand the code](./02-understand.md).
