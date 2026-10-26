# fn-generators

> Use a generator when callers benefit from consuming values one at a time.

## Why it matters

Generators can avoid holding a full result in memory, but they are lazy, usually one-shot, and keep their local state until iteration ends.

## Guidance

Return a concrete collection when callers need indexing, repeated traversal, or a stable snapshot. Use a generator when callers can consume values one at a time and benefit from avoiding a full intermediate collection. Document yielded values when that is not clear from the signature. Do not let a generator's lifetime accidentally control an open file or other resource.

For example, a report operation that returns a count does not need to build a temporary list before summing it. The public result stays an integer in both versions.

## Bad

```python
from collections.abc import Iterable


def total_characters(lines: Iterable[str]) -> int:
    lengths = [len(line) for line in lines]
    return sum(lengths)
```

## Good

```python
from collections.abc import Iterable


def total_characters(lines: Iterable[str]) -> int:
    return sum(len(line) for line in lines)
```

## Caveats

A generator changes when its body runs, how many times callers can traverse the result, and when iteration errors or side effects occur. Keep a collection return type when callers need that contract. A generator can own a resource when it has a clear close path and callers use it within a context manager. Prefer a context-managed API when early termination must release the resource promptly.

## Related rules

- [Context managers](res-context-managers.md)
