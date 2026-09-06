# Understand the code before changing it

Editing code you don't understand is how subtle regressions ship. bstack gives you two ways in. `/how` explains what the code does now. `/teach` builds that understanding into a plain explanation.

## Trace behavior with `/how`

```text
/how do we dedupe notifications? is there an n+1 when we look up subscribers?
```

Ask the question you actually have. [`/how`](../../skills/how/SKILL.md) reads the code and answers at the level of a senior engineer onboarding you onto the subsystem, with the runtime flow, the key types, and the non-obvious parts. For a big subsystem it fans out two to four read-only explorers first. For a narrow question it just reads and explains.

`/how` can also push back on the design. Ask for Critique mode when you suspect the structure itself:

```text
/how explain the sync service, then critique its ownership boundaries
```

The explanation comes first, so the critique stays grounded in how the thing really works.

## Actually understand it with `/teach`

```text
/teach me how this PR changes retries. convince me it fixes the cause and not the symptom.
```

[`/teach`](../../skills/teach/SKILL.md) is for when a summary isn't enough. It uses `/how` and weaves the findings into a plain explanation that builds up diagram by diagram. The "convince me" framing is worth stealing. It turns the explanation into an argument you can poke at instead of a tour.

**Pitfall:** don't skip this page's skills because "the agent will read the code anyway." An agent that starts editing without a traced model tends to fix the symptom at the first plausible spot. `/how` first is cheaper than the second bug.

Next: [Design the change](./03-design.md).
