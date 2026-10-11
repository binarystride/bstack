# Laziness Protocol

Writing code is cheap for you, which makes over-engineering easy. Counter it by borrowing a human maintainer's fatigue. Aim for the most result with the least code and complexity.

- **Prefer deletion.** Apply [subtract-before-you-add](subtract-before-you-add.md) before introducing a new mechanism.
- **Minimize the diff.** Make the smallest change that solves the observed problem. Do not broaden the task merely because adjacent code could be improved.
- **Question the threading.** If a task asks you to pass a new signal through types, schemas, pipelines, or similar layers, stop and look for a more direct path.

For simplification, apply [Code Simplification](code-simplification.md). It owns behavior preservation, clarity, conventions, scope, and incremental verification. For decisions about extracting, retaining, or removing a module, apply [Codebase Design](codebase-design.md). Do not flatten a useful module merely because its implementation spans several files.

**Prime directive:** If a human developer would find the code exhausting to maintain, it is a bad solution. Be lazy. Stay simple.
