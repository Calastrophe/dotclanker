---
name: python
description: Use when writing, reviewing, or refactoring Python code.
---

# Python

Follow the repository's Python version, configuration, and existing conventions first. Use these rules as portable defaults when the project has not chosen differently. Keep edits within the requested scope and avoid unrelated rewrites. Read only the rule files relevant to the code or review.

Examples use Python 3.10 or later. Adapt syntax and standard-library APIs to the project's declared minimum version.

## Tooling

- [tool-uv](rules/tool-uv.md) - Prefer uv for new projects; preserve the package manager selected by an existing project.
- [tool-pyright](rules/tool-pyright.md) - Use Pyright for static type checking when the project has not selected another checker.

## Imports and errors

- [org-imports](rules/org-imports.md) - Make import sources clear and follow the package's import convention.
- [org-package-layout](rules/org-package-layout.md) - Preserve package boundaries and use the project's supported invocation path.
- [err-exceptions](rules/err-exceptions.md) - Raise and catch exceptions where failures can be explained or handled.
- [err-assertions](rules/err-assertions.md) - Reserve assertions for internal invariants, not required input checks.

## State and functions

- [state-default-args](rules/state-default-args.md) - Avoid mutable and time-dependent function defaults.
- [state-global](rules/state-global.md) - Keep mutable state owned by an object or operation with a clear lifecycle.
- [fn-comprehensions](rules/fn-comprehensions.md) - Use comprehensions when their clauses remain readable.
- [fn-iterators](rules/fn-iterators.md) - Prefer direct iteration and membership operations over temporary lists.
- [fn-generators](rules/fn-generators.md) - Use generators when lazy, one-pass consumption helps callers.
- [fn-lambda](rules/fn-lambda.md) - Keep lambdas short and easy to understand at the call site.
- [fn-closures](rules/fn-closures.md) - Keep captured state clear, especially in callbacks created in loops.
- [fn-conditionals](rules/fn-conditionals.md) - Keep branches and truth-value distinctions explicit.
- [fn-local-helpers](rules/fn-local-helpers.md) - Split functions when a named boundary makes the flow clearer.

## Types and classes

- [type-containers](rules/type-containers.md) - Choose collections by their operations and use data classes for plain records.
- [type-annotations](rules/type-annotations.md) - Annotate interfaces and non-obvious internal contracts using supported syntax.
- [class-properties](rules/class-properties.md) - Keep properties cheap and predictable; use methods for visible work.
- [class-decorators](rules/class-decorators.md) - Use decorators for clear, focused transformations.
- [class-super](rules/class-super.md) - Use cooperative `super()` calls in inheritance hierarchies.

## Resources and concurrency

- [res-context-managers](rules/res-context-managers.md) - Give acquired resources an explicit cleanup boundary.
- [conc-threads](rules/conc-threads.md) - Make thread state, synchronization, and worker lifetime explicit.
- [conc-async](rules/conc-async.md) - Keep blocking work off the event loop and await relevant tasks.

## Text and formatting

- [str-formatting](rules/str-formatting.md) - Choose string construction that keeps values and costs clear.
- [log-calls](rules/log-calls.md) - Pass values to logging calls without eager message formatting.
- [fmt-layout](rules/fmt-layout.md) - Follow the repository formatter and formatting settings.
- [name-conventions](rules/name-conventions.md) - Use conventional names that explain each value's role.
- [doc-docstrings](rules/doc-docstrings.md) - Document public contracts and non-obvious behavior.
- [script-main](rules/script-main.md) - Keep imports safe and put executable behavior behind a main guard.
