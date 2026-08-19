### Shipping

**You own what lands. Verify each PR independently, land only the verified run from the root, then keep your hands off the queue.** For "land the stack", "ship it", "enable merge when ready", or the second half of a stack that **Babysit** already drove to green.

This is the half after `playbooks/babysit.md`. Babysit makes a stack mergeable. Shipping decides what is actually safe to merge, then lands it one PR at a time from the bottom. Green is not safe, and the gap between those two words is where this playbook lives.

A stack here is plain git and plain GitHub: each branch is cut from the one below it, and each PR targets its parent branch rather than trunk. Only the root targets trunk. If your team uses a stacking tool, let it do the mechanics below and keep the gates.

1. **Verify every PR independently before landing anything.** One subagent per PR, not batched, remote if your harness runs remote agents, each exercising the real surface (**control-ui** or **control-cli** as the change demands) against parent versus head. Each returns `PASS`, `PASS+NOTES` or `FAIL` and posts that verdict on its own PR so the record outlives the chat. Safe means a verdict from an agent that did not write the code. CI green is not a verdict, and an approving bot review is not a verdict.
2. **Land only the contiguous verified run rooted at the bottom.** Walk up from the lowest unmerged PR and stop at the first one without a passing verdict, where both `PASS` and `PASS+NOTES` pass. A verified PR sitting above an unverified one is not landable, because merging it would pull the gap in underneath it. Report the ceiling as a PR number and say what breaks the chain.
3. **Re-check that the verdicts still describe the code.** A rebase rewrites every SHA above it and silently invalidates every verdict without touching a single check. Compare `git patch-id` at the verdict SHA against the current head before trusting an older verdict, and re-verify anything that actually drifted. Twenty-one verdicts went stale this way in one run with no signal at all.
4. **Never enable GitHub auto-merge on a stacked PR.** Only the root targets protected trunk. Every child targets its unprotected parent branch and already reads `CLEAN`, so GitHub would merge children into parents immediately and collapse the stack into itself. If a previous agent armed it, disarm with `gh pr merge <n> --disable-auto` and confirm the field is back off. Auto-merge is safe on exactly one PR at a time: the root, once it is the only thing targeting trunk.
5. **Land the run bottom-up, one PR per cycle.** Merge the root, then let the next PR become the new root before touching it:
   ```bash
   gh pr merge <root> --squash --delete-branch
   ```
   Deleting the merged branch is what makes GitHub retarget the child PR onto trunk. Confirm that retarget landed with `gh pr view <child> --json baseRefName` before doing anything else; a child still pointing at a deleted branch is a stalled stack, not a merged one.
6. **Rebase the new root only if it needs it, then re-verify what moved.** If trunk moved under it, rebase the branch onto trunk and force-push with `--force-with-lease`. That rewrites the SHA, so step 3 applies again to that PR and every PR above it. A clean fast-forward changes nothing and needs no re-verification.
7. **Wait for the new root's checks before the next merge.** Its checks re-run against the new base and a merge that was green against the old parent can fail against trunk. Poll with the watcher from `playbooks/babysit.md` step 6, not with a sleep loop. Only merge when GitHub itself says the PR can merge.
8. **One merge in flight, always.** Never merge two PRs of the same stack concurrently, and never push to a branch that is mid-merge. Repeat steps 5 through 7 until the run is landed or a PR stops being mergeable. If a cycle stalls, diagnose before mutating, because a stalled retarget and a broken stack look identical from the outside. Report each merge and the new ceiling as you go, so an interrupted run can be resumed from the record rather than re-derived.
9. **Stop at the ceiling.** When the verified run is merged, report what landed, what the next unverified PR is, and what verifying it would take. Extending the run is a new pass through step 1, not a judgment call you make at 3am.

**Reply:** the verified run and its ceiling, each PR's verdict and who produced it, what landed and in what order, any PR that needed a rebase and was re-verified, and what the next gap needs.
