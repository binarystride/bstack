---
name: show-me-your-work
description: "Keep a reviewable decision trail for long-running or unattended work: a TSV log with one row per decision (what, why, evidence, result). Local by default; commit it when a reviewer needs the trail to trust the result. Use for /show-me-your-work, autonomous or multi-phase runs, or work a human reviews after stepping away."
disable-model-invocation: true
---

# Show me your work

For work a human reviews after the fact, a decision trail lets them reconstruct what was decided, why, and on what evidence, without rerunning the work or reading the whole transcript. Keep one canonical log so the trail is consistent and a future agent can find it.

## The format

A single TSV file, one row per decision. TSV because GitHub renders it as a sortable table, `column -s$'\t' -t` and spreadsheets read it, and a row appends with one command. Cells stay single-line. Evidence is a pointer, not prose.

Copy `references/decision-log-template.tsv` (the header row) to start a clean log. Columns:

- **ts.** ISO8601 timestamp. The timeline axis.
- **phase.** The phase or workstream.
- **decision.** What was chosen or done, one line.
- **why.** The reason in plain words. If a principle drove it, say it plainly (`explored options first, this was a one-way door`), not as a jargon tag.
- **evidence.** A link or path that proves it: commit SHA, PR number, `file:line`, or an artifact, trace, or screenshot path. Never a paragraph.
- **result.** The outcome or predicate state: `tests green`, `reverted`, `pixel-diff 0`, `INCONCLUSIVE`, `open`.

An example, plain-spoken so a reviewer reads it at a glance. This is illustration only; don't copy these rows into a real log.

```
ts	phase	decision	why	evidence	result
2026-05-24T09:02:00Z	frame	counted the work first, about 100 components and roughly 75 hours	wanted to know the size before starting a long run	commit 3a9f1c2	found 5 things to sort out before starting
2026-05-24T09:40:00Z	harness	took screenshots of the old version before changing anything	so we can compare old against new and catch any visual change	scripts/snapshot.sh, baseline/	saved 120 reference screenshots
2026-05-24T11:15:00Z	widget	moved the widget styles over without changing how it looks	keep the change small and the result identical	commit 7c21e0a, pixel-diff 0	looks identical, tests pass
2026-05-24T12:30:00Z	widget	threw out a helper's work because its screenshots were blank	checked the real files instead of trusting its summary	worktree reset	reverted, tightened the instructions for next time
```

## Logging a row

Write each entry the way you'd tell a teammate what you did. Plain words, concrete actions, no AI speak or abstract jargon (the **unslop** skill applies to log text too). A reviewer should understand each row without decoding it.

Use the helper so rows stay well-formed: `<skill-dir>/scripts/log.sh <logfile> <phase> <decision> <why> <evidence> <result>`, where `<skill-dir>` is the directory you read this skill from. It stamps `ts`, writes the header on first use, strips stray tabs/newlines, and prefixes any cell starting with `=`, `+`, `-`, or `@` with a single quote so a reviewer opening the log in a spreadsheet doesn't trigger formula execution. A bare `printf` appending a row works too, but mind those same bytes if cells come from generated or user-supplied text.

Write the row when the decision happens, never in a batch at the end. On a long run your context gets compacted and the reasoning is gone; the log is what survives that, which is the entire reason it exists.

Log decision points and checkpoints, not every action: a fork chosen, a unit completed with its verification result, a pivot or revert with its trigger, a blocker surfaced, a gate fixed. For loop runs, one row per iteration. Skip the trivial and self-evident.

## Where it lives

By default the log is a working artifact, not committed. Keep it at `decisions.tsv` in the work dir, or `.audit/<task-slug>.tsv` when several efforts run at once, and leave it out of git. Most work doesn't need a committed trail; the local log still keeps the run honest and can be discarded after.

Commit it only when the work is ambitious enough that a reviewer needs the trail to trust the result: a large cross-language port, a multi-week migration, anything where confidence has to be shown rather than assumed. A committed log renders as a table in the PR.

## Rules

- One row is one decision or checkpoint. If it doesn't fit on one line, the decision isn't crisp yet.
- Append-only. A wrong call gets a new row that supersedes it. Never edit or delete history.
- Prefer evidence produced by committed scripts over hand-made one-offs, so a reviewer can re-run it (the **encode-lessons-in-structure** principle skill).

## Audit the log against the durable record

At the end of the run, before handing back, check the log told the truth.

Audit it against what the run actually left behind, not against your memory of it. By the time a long run ends, the early decisions are outside your context, and a self-audit from memory rubber-stamps exactly the rows most likely to be wrong. The durable record is what you can still open: `git log` and `git diff` on the branches you touched, the commits and their contents, open PRs and their CI results, files on disk, and every artifact path a row points at.

That is also why the `evidence` column is a pointer rather than prose. Auditing is resolving those pointers.

- **Every row resolves.** Open each evidence pointer. A SHA that isn't in the history, a `file:line` that doesn't exist, a screenshot path with no file: the row is wrong or the work didn't happen. Cut it or correct it.
- **Every pointer shows what the row claims.** A commit that exists but doesn't contain the change described is a worse failure than a missing one, because it reads as verified.
- **The record has no unlogged forks.** Walk the commits and branches. A revert, an abandoned branch, or a reset with no matching row is a gap. Add the row from what the record shows.
- **A row with no resolvable evidence is not a decision, it's a claim.** Either find the evidence or mark the result `unverified`. Never leave it looking proven.
- **Drop padding.** If nobody would audit a row, it doesn't earn its place.

## Cross-model review of the trail

Before handing back, you must spawn a subagent on a different model family from the one that did the work. Self-review is not a substitute; the point is fresh eyes you cannot bring yourself.

Give it the log and the durable record: the log file path, the branches and commits the run produced, the PRs it opened, and the artifact paths the rows point at. Do not try to hand it your transcript. A subagent cannot read its parent's session in most harnesses, and the record is the stronger evidence anyway because it is what a human reviewer will also see.

It flags what the user should pay attention to. Not a redo of the work, a scan for what's suboptimal or risky.

- Decisions logged with weak or absent evidence.
- Verification steps claimed in the log with nothing in the record that proves them.
- Choices that look risky in hindsight (premature, scope-creeping, papering over a symptom).
- Gaps the user would otherwise miss on a casual skim.

Every reply for a run that produced a trail ends with an "Attention" section. Lead with the reviewer's model on its own line (`reviewed by <model>`), then list each flag pointing to specific rows or moments. "No flags" is a valid value; the model name is not. The self-audit asks if the log told the truth; this asks what the user should still scrutinize even when it did.

## Reviewing the trail

Read top to bottom, follow the evidence pointers, spot-check. GitHub renders a committed TSV as a table; `column -s$'\t' -t decisions.tsv` renders it in a terminal. A row whose evidence doesn't resolve, or whose result is unverified, is the audit catching a gap.

## Composing this skill

Other skills route their audit trail here instead of inventing one. Reference it by name and let it own the format; don't restate the columns.
