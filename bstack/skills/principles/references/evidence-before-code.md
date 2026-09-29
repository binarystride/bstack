# Evidence Before Code

Write code only for what you have observed. Every branch, guard, retry, fallback and fix traces to evidence: a production count, a log line, a vendor document, a spec rule, or the requirement itself.

**Why:** A plausible failure is easy to imagine and expensive to carry. Code for a case that never happens still has to be read, tested and maintained, and it hides the cases that do happen. Reasoned claims about data are often wrong, and counting the real records settles them in minutes.

**Pattern:**

- Before you handle a case, count it. Show the query and the number.
- A case you looked for and did not find goes in the PR description under "Not handled", with how you looked. It does not go in the code.
- Before you fix a review finding about existing data or state, count how often that input occurs. If it never occurs, reply with the count instead of changing code.
- Label every claim you report: **confirmed** (you checked it, and say how), **inferred** (you reasoned it, and say from what), or **unverified**. Never present an inference as a fact.
- A test fixture proves that a code path exists. It does not prove that the data occurs.
- A count settles questions about inputs that already exist. For a state, race or partial failure that the new code itself creates, judge from the code and the upstream contract (documented retries, ordering, limits). Production has no record of it yet.

**Boundaries:**

- Validation at trust boundaries stays whether or not an attack has been seen. See [boundary-discipline](boundary-discipline.md).
- Error handling that prevents losing data or money stays.
- When you cannot measure (no access, no history), say so, mark the claim unverified, and ask or list the question. Do not fill the gap with an assumed case.
