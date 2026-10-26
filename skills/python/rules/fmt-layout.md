# fmt-layout

> Let the repository's formatter and configuration decide routine layout.

## Why it matters

Consistent formatting makes diffs smaller and lets reviewers focus on behavior instead of personal whitespace preferences.

## Guidance

Check for an existing formatter, lint configuration, and line-length policy before editing. Use that tool for changed Python code. When the project has no rule, follow PEP 8: use four-space indentation, keep related expressions together, and break long expressions at clear syntactic boundaries. Avoid semicolon-separated statements and backslash line continuations when parentheses can express the same layout.

## Bad

```python
if enabled and ready and available and not blocked: run_job()
```

## Good

```python
if enabled and ready and available and not blocked:
    run_job()
```

## Caveats

Generated files and data formats may have their own layout rules. Do not reformat unrelated files or override a project's formatter with personal preferences.
