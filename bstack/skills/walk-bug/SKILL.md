---
name: walk-bug
description: Investigate one reported bug and explain it the way a good PR description explains a change, with a one-sentence verdict, one to three things to note, and a compact outline of the failure path drawn as call trees, pseudocode, sequence diagrams, or diffs instead of prose. Then recommend the fix, wait for the decision, and change only what is approved. Use when the user reports a bug, pastes an error, a Sentry issue, a ticket, or a customer complaint and wants to understand what actually goes wrong before anything is changed. Triggers on "walk me through this bug", "what is going on here", "why does this happen", "investigate this", "find out why".
---

# Walk through a bug

The user has a symptom and does not yet know the cause, how bad it is, or the right fix. Find out, then explain it in the shape below: the shortest account that lets a reader see the whole failure path at a glance. Wait for the decision, then act on that bug only.

`walk-me` takes a list of findings someone already produced. This skill starts from a symptom and produces the finding. Once the user has several bugs from this skill, `walk-me` can take them from there.

## Before you present anything

1. **Pin the symptom.** Restate it in one line to yourself: who sees what, where, since when. Pull the exact error text, the failing request, the order or record id, whatever the report gives you. If the report gives nothing concrete, get the concrete thing first: the log line, the row, the reproduction.
2. **Trace the path.** Start at the point where the symptom shows and follow the code back to the decision that produced the wrong result. Read the actual code at each step. Stop when you can describe the full path from trigger to symptom without guessing any step.
3. **Prove the cause.** Reproduce it, or find the evidence that rules out the other explanations: the log with the wrong value, the row in the wrong state, the test that fails on the base. Never present a cause you have not confirmed. If you cannot confirm it, say so in the writeup and present the leading candidate as a candidate.
4. **Find the reach.** How many users, orders, or runs does it hit, and how often. Measure it if you can. If you cannot, say "unmeasured".
5. **Date it.** `regression` when the behaviour worked before and a change broke it, and name the change. `always` when the code never handled this case. `unknown` when you could not tell. A regression is never optional to fix; an always-was defect is a scope decision.

Do the reading for this bug only. Do not audit the surrounding code. If you notice a second bug on the way, note it in one line at the end and leave it.

## How to present it

Read `references/views.md` for the drawing conventions before writing.

Keep the whole thing under about 250 words plus the drawings. Use this shape and only this shape:

**What goes wrong.** Exactly one sentence, in plain language. Lead with the consequence for the customer where there is one, otherwise for the system. Name the cause in the same sentence if it fits.

**Things to note.** One to three bullets. Only what changes the decision: how many are affected, since when, whether it is a regression and from what, data already written wrong, a second bug found on the way, a fix that would be wrong for a non-obvious reason. Write `- None.` if there is nothing.

**Failure outline.** The smallest set of views that shows the path from trigger to symptom. Pick from `references/views.md`. Mark the step where it goes wrong. Show what actually happens, not what should happen, unless a two-column contrast is the fastest way to land it. Typical choices:

- A call tree from the entry point to the line that misbehaves, with the failing step marked.
- Pseudocode of the branch that takes the wrong turn.
- A sequence diagram when the bug lives between two systems or in ordering.
- A file tree when the reader needs to know which module owns the decision.
- A small `diff` of a data shape or a row when the bug is in what got stored.

Put one short line of text above each view saying what it shows. Order the views in the order things happen.

**Where.** File and line, function name, or the exact error text. Enough to open it, nothing more.

**Origin.** `regression` (name the change), `always`, or `unknown`.

**The fix.** One or two sentences, then a `diff` of the change if it fits in a few lines. Show the fix as a diff of pseudocode or the real code, whichever is shorter and still exact. If the smallest fix and the root-cause fix differ, show the root-cause fix and say why.

**Alternatives.** Only if there is a real choice. One line each, at most two, and say which you recommend and why. If one fix is obviously right, propose nothing else.

End with a direct question: go ahead, pick an option, or leave it.

## When the report is wrong

Say so plainly and give the one fact that shows it: the code path that handles the case, the log that shows the expected value, the setting that explains what the user saw. A report that turns out not to be a bug is a valid outcome. Recommend closing it and ask the user to confirm.

## When you cannot settle it

If the evidence does not single out one cause, present the candidates in the failure outline, at most three, each with the one thing that would confirm or rule it out: a production query, a log line to add, a supplier answer, a reproduction with a specific input. Say which candidate you rank first and why. Propose the next step and offer to do it now if it is something you can do.

Never invent confidence to fill the gap.

## After the user decides

- **Fix it now.** Make the smallest change that fixes the root cause. Then run the narrowest check the project already has for that code: the one test file, the type check. Not the full suite. Add a test only where the project's testing rules would ask for one. Report the result in one line.
- **Leave it.** Record the decision if it is durable, meaning a future session would otherwise re-investigate: a deliberate gap, an accepted rate, a rule. Write it wherever this project keeps durable decisions, and if it keeps none, say so and put it in the reply.
- **Investigate further.** Do the one step agreed, then present again in the same shape with what changed.

Do not commit, push, open a PR, or create a ticket unless the user says to. Offer, with the project's own conventions worked out: commit message format, formatting step, branch, whether a ticket or Sentry issue should be referenced.

## Never

- Never present a cause you have not confirmed as if it were confirmed.
- Never describe the path in prose when a call tree or diff would show it in fewer lines.
- Never draw a view that does not carry the failure. Every view must contain the marked step or the changed line.
- Never fix anything before the user decides.
- Never pad. If a sentence does not change what the user decides, cut it.
- Never use metaphors, analogies, or invented compound adjectives. Plain sentences, active voice, one idea each.
