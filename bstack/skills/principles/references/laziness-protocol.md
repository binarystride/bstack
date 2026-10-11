# Laziness Protocol

Writing code is cheap for you, which makes over-engineering easy. Counter it by borrowing a human maintainer's fatigue. Aim for the most result with the least code and complexity.

- **Prefer deletion.** When asked to refactor or improve, look for removals before additions.
- **Maintain a flat call hierarchy.** Avoid deep call chains. A rich interface that hides substantial work is not a deep call chain. If answering a question requires tracing through more than 3 files or layers, flatten it.
- **Consolidate decisions.** Do not repeat the same choice in several places. Put it behind one source of truth and pass the result as a simple flag.
- **Minimize the diff.** Make the smallest change that solves the problem. Judge simplicity by what a reader must understand, not by line count. Keep explicit returns when merging branches would add state the reader must track.
- **Preserve behavior when simplifying.** Keep inputs, outputs, errors, side effects, and their order unchanged. If equivalence is unclear, leave the code alone. A shorter expression is not worth a new mental step.
- **Question the threading.** If a task asks you to pass a new signal through types, schemas, pipelines, or similar layers, stop and look for a more direct path.
- **Sweat the small leaks.** Remove tiny pass-throughs, representation leaks, and duplicated choices before they spread. Small leaks compound into permanent coordination costs.

**Prime directive:** If a human developer would find the code exhausting to maintain, it is a bad solution. Be lazy. Stay simple.

The behavior-preserving simplification guidance draws on [Addy Osmani's Code Simplification](https://github.com/addyosmani/agent-skills/blob/9d0c60d406b454a78ccc0a175b19932047aa4dac/skills/code-simplification/SKILL.md). This principle is self-contained; the source skill is not required.
