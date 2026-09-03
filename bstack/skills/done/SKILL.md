---
name: done
description: "Answer \"are we done here, can I archive this thread\". Scans the thread for unfinished work, checks the durable record instead of trusting recall, and reports a short done summary plus what is still open and who owns it. Use before archiving or closing a chat."
---

# Done

Answer one question: can this thread be archived?

## Steps

1. **Re-read the thread for open items.** Look for: work you proposed and never did, questions the user asked that you never answered, things you said you would follow up on, decisions parked as "later" or "TODO", findings discovered mid-task and set aside, and chores that are yours rather than the user's (update a ticket, update a doc or status page, resolve PR review threads, update a PR description, write a memory).
2. **Check the record, don't trust memory.** A long thread has compacted away the early rows, and a self-audit from recall rubber-stamps exactly what it forgot. Verify anything cheap to verify: `git status --short`, unpushed commits, `gh pr view --json state,reviewDecision,mergeStateStatus`, unresolved review threads, whether the file you said you would edit actually changed. Spend seconds here, not minutes.
3. **Drop what does not matter.** An item belongs in the report only if the user would regret losing it. Speculative ideas, nice-to-haves nobody asked for, and work the user already declined are not open items.
4. **Report.**

## Report

Verdict on line one: **archive** or **not yet**, plus the reason in under ten words.

**Done.** At most five lines, one clause each. Enough to recognise the thread, never a recap. The user does not want to re-read the work.

**Open.** One line per item: what it is, who owns it (`you` or `me`), and whether it blocks archiving. Sort blockers first. If the list is empty, say so in one line.

Nothing else. No summary paragraph, no next-steps essay.

If any open item is mine and takes one step, offer to do it now in a single line. Do not do it unasked, and never create a ticket or a PR without being told to.
