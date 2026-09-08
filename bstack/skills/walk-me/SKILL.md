---
name: walk-me
description: Work through a list of reported problems one at a time, each with a short plain explanation, why it matters, and a recommended fix, pausing for the user's decision before touching anything. Use after a code review, audit, agent report, or investigation produced findings and the user wants to go through them one by one and fix as they go, rather than read one long report. Triggers on "work through these", "one by one", "go through the findings", "let's fix these one at a time", "walk me through the issues".
---

# Work through problems one at a time

The user has a list of reported problems and does not yet know which are real, which matter, or what the right fix is. Take them one at a time. Explain briefly, recommend, wait for the decision, then act on that one problem only.

The findings usually come from the **review** or **interrogate** skill. Where `/review --fix` applies everything above P3 on its own, this walks the list with the user and changes only what they approve.

## Before the first problem

Read the findings and the code they point at. If a finding is vague, verify it yourself before presenting it. Never present a finding you have not checked.

Then decide the order and the grouping:

- Group problems that block each other, share a root cause, or where fixing one changes the answer for another. Present the group as one item with one decision.
- Order by dependency first, then by severity.

Open with one line: how many items there are and how they are grouped. Nothing else. No summary of the findings, no preamble.

## The loop

For each item:

1. **Do the cheap homework first.** If a few minutes of reading code, docs, logs, or data would settle the question, do it now, for this item only. Never gather everything for every problem up front.
2. **Present it** in the shape below, then stop.
3. **Answer follow-up questions** in the same tight style. Then ask for the decision again. Do not move on until the user has decided.
4. **Act on the decision**, for this item only.
5. **Go to the next item.** Do not commit, do not push.

## How to present one item

Keep the whole thing under about 150 words. Hard limits:

- What goes wrong: one or two sentences, in plain language. Lead with the consequence for the customer where there is one, otherwise for the system.
- Why it matters: one sentence. Say honestly how bad it is and how often it happens. If you do not know how often, say so.
- Where: file and line, function name, or the exact error text. Enough to look at it, nothing more.
- Origin: one of `new in this change` or `already on the base branch`. Fixing a regression is rarely optional; fixing an old defect is a scope decision. If the findings do not say, check whether the failing line is in the diff against the base before presenting the item.
- The fix: one or two sentences.
- Alternatives: only if there is a real choice to make. One line each, at most two of them, and say which you recommend and why. If one fix is obviously right, propose nothing else.

Number the items and any options inside them, so "option 2" is unambiguous later.

End with a direct question: go ahead, pick an option, or skip.

Then the progress line, always the last line of the message:

`Progress: ✓ ✓ ✗ **P2** [P3] [P3]`

- `✓` fixed, `✗` skipped or left as-is.
- Undecided items show their severity. A grouped item takes the highest severity in the group.
- `[ ]` around an item you recommend leaving. Drop the brackets once it is decided.
- Bold the item on the table now.

Items stay in presentation order, so the line reads left to right as done, current, remaining.

## Items that need no fix

Say so plainly and give the one reason. A reported problem that is wrong, already handled elsewhere, or too rare to be worth the code is a valid outcome. Recommend leaving it, and ask the user to confirm.

## Items you cannot settle yet

If you are not confident enough to recommend a fix, say exactly what is missing and what would settle it: a production measurement, a supplier answer, a document to read. Propose the next step, and offer to do it now if it is something you can do.

Never invent confidence to fill the gap.

## After the user decides

- **Fix it now.** Make the smallest change that fixes the root cause. Then run the narrowest check the project already has for that code: the one test file, the type check. Not the full suite.
- **Defer it.** Record the decision if it is durable, meaning a future session would otherwise re-litigate it: a rejected approach, a deliberate gap, a rule. Write it wherever this project keeps durable decisions, and if it keeps none, say so and put it in the reply. Skip this for routine fixes; the diff records those.
- **Leave it.** Say it is closed and move on.

Report the result in one line before moving to the next item.

## When every item is done

Give the recap:

1. The finished progress line: `Progress: ✓ ✓ ✗ ✓ ✗ ✗`.
2. One line per item: number, severity, short title, origin, outcome, and the one fact that explains the outcome. Where a deferral was recorded, say where.

`1. P1 Stale reprice reverts the booking — new — fixed, guard in the shared reprice path.`
`3. P2 Duplicate webhook delivery — pre-existing — deferred, needs a prod count first. Recorded in DEFERRED.md.`
`5. [P3] Retry on 429 — pre-existing — left, too rare to be worth the code.`

Then offer to commit and push, following the project's own conventions. Work out what applies here: the commit message format, the formatting step, the branch, whether a PR exists and whether its description needs updating. If the findings came from PR review threads, offer to resolve those threads after pushing.

Offer. Do not commit, push, open a PR, or create a ticket unless the user says to.

## Never

- Never present the whole list of problems in detail up front.
- Never fix an item the user has not decided on.
- Never pad. If a sentence does not change what the user decides, cut it.
- Never use metaphors, analogies, or invented compound adjectives. Plain sentences, active voice, one idea each.
