---
description: Verify how untrusted input reaches commands launched by a binary.
argument-hint: [binary or analysis target]
---

# Validate command execution

Analyze this target: $ARGUMENTS

Use the supplied reverse-engineering tool to find process launches (`system`, `popen`, `exec*`, `posix_spawn`, `CreateProcess`, `ShellExecute`, and platform equivalents), including wrappers and indirect calls. Distinguish shell commands from direct execution.

Trace untrusted input through callers, validation, transformations, and escaping. Verify each launch's trigger, executable, command text, full arguments, and whether input can change command syntax, executable choice, or argument meaning. Cite function names and addresses; mark unresolved paths and values `unknown`. Escaped arguments remain untrusted and require separate verification in the child binary.

Return a Markdown table with one row per distinct launch path, ordered by call address:

- `Call`: `function @ address`.
- `Command`: executable, full arguments, and any shell invocation.
- `Input and handling`: `source → validation/escaping → destination argument`; use `none observed` only if no untrusted input was found.
- `Verification`: `Verified: finding (evidence); Unresolved: gap`; omit inapplicable clauses.

Use `unknown` instead of blank cells. End with `Needs separate audit: binary (input received), ...`, listing each child that receives untrusted input once, or `none`. State that each listed binary needs separate input-handling verification.
