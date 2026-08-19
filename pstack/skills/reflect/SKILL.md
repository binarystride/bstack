---
name: reflect
description: Spawn three parallel review subagents over the active transcript, surface learnings, and route each to a concrete edit on an existing skill. Use when the user says reflect.
disable-model-invocation: true
---

# Reflect

Mine the current conversation for durable learnings, then route them into skill edits.

## When to invoke

- The user said "reflect" or "/reflect".
- A complex task (5+ tool calls) just landed cleanly and the recipe is worth keeping.
- The agent hit dead ends, found the working path, and the path generalizes.
- The user corrected the agent's approach mid-task.
- A non-trivial workflow emerged that isn't captured anywhere.

Skip when the conversation is trivial, off-topic, or already covered by an existing skill the parent followed correctly. One-offs are not learnings.

## Process

### 1. Locate the active transcript

The parent finds its own transcript file before fanning out.

Transcripts are JSONL, one chat message per line, but where they live differs by harness and changes over time, so discover the path rather than construct it. If your system prompt names the transcript directory, use that. Otherwise check the known roots (`~/.claude/projects/`, `~/.cursor/projects/`, `~/.codex/sessions/`) and find the entry for the active workspace. Naming varies: some harnesses key the directory by the workspace path with every `/` turned into `-`, and they disagree on whether the leading slash survives; others partition by date with no workspace key at all. Match what is on disk and confirm you found real transcripts before mining them.

Stay inside the current workspace's transcripts. Globbing across every project reads private chats from unrelated work.

```bash
ls -t <agent-transcripts>/*.jsonl <agent-transcripts>/*/*.jsonl <agent-transcripts>/*/subagents/*.jsonl 2>/dev/null | head -10
```

Three transcript layouts: legacy flat (`<id>.jsonl`), current nested (`<id>/<id>.jsonl`), and subagent (`<parent>/subagents/<child>.jsonl`).

For each candidate, read the first JSONL line and check that `message.content[0].text` contains the conversation's opening user prompt. Take the matching path. If no path resolves, write a tight digest of the session and pass that instead.

### 2. Spawn three reviewers in parallel

One message, three general-purpose subagents. Reviewers need MCP access for context lookups (tickets, chat threads, observability traces named in the transcript), so do not put them in a restricted mode that strips MCP tools. The prompt forbids file writes; the parent applies edits.

| Lens | Model | Prompt template |
|---|---|---|
| Judgment | your strongest judgment model | `references/judgment-reviewer.md` |
| Tooling | a different family from the judgment lens if you can reach one | `references/tooling-reviewer.md` |
| Divergent | your strongest judgment model | `references/divergent-reviewer.md` |

Pass each template verbatim, substituting the transcript path or digest where marked. Reviewers return findings in their response body.

### 3. Synthesize

One general-purpose subagent on your strongest judgment model. Its quality check spot-verifies citations, which can require MCP access, so keep its tools unrestricted. Use `references/synthesizer.md` verbatim, with each reviewer's full output inlined where marked. The synthesizer returns a structured Accepted / Rejected / Backlog list.

### 4. Structural enforcement check

Sanity-check the synthesizer's Accepted list. For any item that would be enforced more reliably by a lint rule, script, metadata flag, or runtime check, move it from Accepted to Backlog. The synthesizer already applies this criterion; this is a final pass before edits land. See the **encode-lessons-in-structure** principle skill.

### 5. Apply

Before applying any Accepted edit, present the synthesizer's full Accepted/Rejected/Backlog output to the user and wait for explicit approval. The user picks which subset to apply and may redirect routings. Skill changes affect every future agent in the org; do not auto-apply.

Backlog items file to whatever devex / backlog tracker your team uses automatically. Those are tracker submissions, not skill edits. Only the Accepted list waits for approval.

For each approved Accepted item, follow the Routing field exactly:

- Trivial existing-skill edit (a one-line bullet, a tightened sentence, a stale fact corrected): parent does directly.
- Substantive existing-skill edit (a new section, a new pattern table, more than ~10 lines): hand to your harness's skill-authoring flow if it has one, and run its draft / test / iterate loop. With no such flow, do it directly, but keep the loop: draft, trigger the skill on a real case, revise.
- `tune description: <skill path>` (the skill exists but didn't trigger when it should have): the fix is the `description` frontmatter, not the body. Rewrite it to name the phrases a user would actually type, then re-test that the skill fires on them.
- `new skill: <kebab-name>`: create it through the same authoring flow. Do not invent the shape ad hoc.

If your environment ships a SKILL.md validator, run it on every touched skill before declaring done. Skip this step if it doesn't.

### 6. Summarize for the user

Short list, no preamble:

- Edits applied: `<skill path>`. What changed, one line each.
- New skills created: `<skill path>`. One line each (rare).
- Backlog filed to the devex tracker: `<issue title>` (`<tags>`). One line each.
- Dropped: one line per rejected finding + reason from the synthesizer.
