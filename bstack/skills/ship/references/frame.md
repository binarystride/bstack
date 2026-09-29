# Frame questions

Answer each question in writing before you design. Keep the answers short and attach the evidence. Skip a question only when it cannot apply, and say why.

## Outcome

- Who uses this change: a customer, staff, a supplier, the team?
- What outcome does the business want? Name the signal that will show it happened.
- What does the ticket ask for, and what problem is behind the ask? The two can differ. Solve the problem.

## Should we build it

- Does the problem exist, and how often? Count it in production.
- Does something already do this, in the codebase or in a service we already pay for?
- Is there a smaller fix outside code: configuration, data, a document, an operations step?
- What happens if we do nothing?

## Patch or redesign

- Does this patch earlier work that was built wrong? Where does the root cause live?
- If we built this today with no legacy, what would it look like? See [redesign-from-first-principles](../../principles/references/redesign-from-first-principles.md).
- How far is that from the current design? Which parts of the greenfield design can we take now without breaking callers or stored data, and what is the migration path?
- Does this change add a special case that the next person must remember?

## Options

- Write two to five structurally different options, not variations of one. See [exhaust-the-design-space](../../principles/references/exhaust-the-design-space.md). Include "do less", and "redesign" when the current design is the problem.
- Compare them on customer outcome, experience, risk to money and data, size of the change, security, maintenance, migration and rollback.
- Pick one. Write why it won and why each other option lost. A rejected option that someone could bring back later is a decision worth recording where the repository keeps decisions.

## Experience

For anything a person sees or does:

- Walk the flow as that person. Is every word needed and clear? Is anything they need in order to act missing?
- Is each piece of information where they will look for it?
- Do we show too much, too little, or anything false?
- Does it work on a phone, with a screen reader, and in every language the product supports?

## Security and operations

- What new input crosses a trust boundary? Who can call this, and with what data?
- What happens on a retry, a duplicate, a partial failure, or events out of order? See [make-operations-idempotent](../../principles/references/make-operations-idempotent.md).
- How will we know it broke in production? Name the log line or monitor.
- What does the next maintainer need to know that the code does not say?

## Challenge your answer

- For each key assumption, write what evidence would prove it wrong. Then look for that evidence.
- Argue for the option you rejected. If the argument holds, switch.
- Imagine this change caused an incident a month from now. What happened? Guard against that only if the evidence says it can happen.

## Finish condition

Write the checks that will prove the work is done: commands, queries, screens. Each one must be able to pass or fail. They go into the PR description before any code, and the Exit phase runs them.
