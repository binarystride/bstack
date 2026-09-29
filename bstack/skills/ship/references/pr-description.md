# PR description

The human reads only this. Write it for someone who has not seen the ticket or the run. Use plain sentences and numbered items, one claim each.

Use these sections, in this order. Leave out a section that would be empty, except "Needs your decision": write "Nothing." there when it is empty.

1. **State.** `READY`, `NEEDS DECISION` or `STOPPED`, and one sentence on why. While the run is still going, write `IN PROGRESS` and the current phase.
2. **What changes.** For the customer and the business first, then for the code. Label each mechanism as existing, added or removed.
3. **Needs your decision.** For each item: the question, the default this PR implements, and what changes if the human picks otherwise.
4. **Decisions made.** Each choice, the options rejected, and the evidence. Put decisions that are hard to reverse first.
5. **Evidence.** The finish condition checks and their results first. Then production counts with their queries, log lines, links to docs and issues, replay results, and what you saw at every place that shows changed data. Mark each claim as confirmed, inferred or unverified. Remove personal and payment details from anything you quote.
6. **For a bug fix.** Which change introduced the bug. Whether it still occurs. Why tests or specs missed it. Which records it affected, including ones still in progress. What corrects itself, and what needs a backfill, a re-send or a manual fix.
7. **Not handled.** Cases considered and not coded, with how you looked for them. Work outside the scope, with links to the follow-up tickets.
8. **Review record.** Each self-review round with the commit it reviewed. Bot runs used, as "n of 5". Findings fixed and findings declined, each with a one-line reason.
9. **Rollout.** Steps a human must take in production, how to see it working or failing, and how to roll it back.

When a flow has more than three steps, add a diagram that the platform renders, such as a Mermaid block on GitHub. Plain text art does not render as a diagram.
