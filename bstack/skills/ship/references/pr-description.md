# PR description

The human reads only this. Write it for someone who has not seen the ticket or the run. Use plain sentences and numbered items, one claim each.

Use these sections, in this order. Leave out a section that would be empty, except "Needs your decision": write "Nothing." there when it is empty.

1. **State.** `READY`, `NEEDS DECISION` or `STOPPED`, the Ship mode (`default` or `fast`), and one sentence on why. While the run is still going, write `IN PROGRESS` and the current phase. For a fast READY, state that the review gate covers P0/P1 and whether any P2/P3 findings are deferred.
2. **Summary.** Two or three sentences a reader can stop after: what was wrong or missing, what this PR does about it, and whether anyone sees different behaviour.
3. **Why.** The background for a reader who has not seen the ticket. What the affected part does and who relies on it. What goes wrong or is missing, and what that costs or risks. Who found it: the ticket, an audit, an alert or this run.
4. **What changes.** What is different after merge, and for whom. Describe it where it shows: what a user sees in the UI, a rule the system now applies, a contract or stored data that changes, a job that runs differently, or how the code is organised. Then say where it applies and what stays the same. Label each mechanism the change relies on as existing, added or removed. Write the intent in words. Show code only when a reviewer must see its exact shape to judge the change, such as a schema, an API contract or a config value.
5. **Needs your decision.** For each item: the question, the default this PR implements, and what changes if the human picks otherwise.
6. **Decisions made.** Each choice, the options rejected, and the evidence. Put decisions that are hard to reverse first.
7. **Evidence.** The finish condition checks and their results first. Then production counts with their queries, log lines, links to docs and issues, replay results, and what you saw at every place that shows changed data. Mark each claim as confirmed, inferred or unverified. Remove personal and payment details from anything you quote.
8. **For a bug fix.** Which change introduced the bug. Whether it still occurs. Why tests or specs missed it. Which records it affected, including ones still in progress. What corrects itself, and what needs a backfill, a re-send or a manual fix.
9. **Not handled.** Cases considered and not coded, with how you looked for them. Work outside the scope, with links to the follow-up tickets.
10. **Review record.** The Ship mode and each self-review round with the commit it reviewed. Bot runs used, as "n of 5". Findings fixed and findings declined, each with a one-line reason. Under `--fast`, list deferred P2/P3 findings with their severity and thread link or self-review reference. Label them deferred and nonblocking, not fixed or disproved; they do not need follow-up tickets or a "Needs your decision" entry unless an explicit user instruction or repository requirement makes them blocking.
11. **Rollout.** Steps a human must take in production, how to see it working or failing, and how to roll it back.

When a flow has more than three steps, add a diagram that the platform renders, such as a Mermaid block on GitHub. Plain text art does not render as a diagram.
