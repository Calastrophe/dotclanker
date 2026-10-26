# type-annotations

> Annotate public interfaces and code where a type clarifies expected values.

## Why it matters

Annotations help readers, editors, and static checkers find mismatches before runtime. They do not validate values at runtime by themselves.

## Guidance

Annotate public function parameters and return values. Add annotations to internal code when they clarify a non-obvious contract or help a checker catch likely mistakes. Skip annotations that merely repeat an obvious local assignment.

Mark optional values explicitly with `T | None`, and use built-in generics such as `list[str]` on supported Python versions. For inputs, choose an abstraction from `collections.abc` when callers may provide different implementations. Use a concrete return type when the function promises that collection. Use a type parameter when it expresses a real relationship between input and output types. Prefer useful types over `Any`.

## Bad

```python
def first(items):
    value = items[0]
    return value
```

## Good

```python
from collections.abc import Iterable, Sequence
from typing import TypeVar

T = TypeVar("T")


def first(items: Sequence[T]) -> T:
    return items[0]


def materialize(items: Iterable[T]) -> list[T]:
    return list(items)


def display_name(primary: str | None, fallback: str) -> str:
    return primary if primary is not None else fallback
```

## Caveats

Validate untrusted input and narrow it with runtime checks. `cast()` only changes what a type checker assumes, and `Any` can hide mismatches.

Use `if TYPE_CHECKING:` for imports needed only by static checking. On Python 3.10 through 3.13, quote those annotation references or use `from __future__ import annotations` to avoid evaluating them when definitions run. That future import stores annotations as strings. Python 3.14 defers evaluation by default.

Neither option makes a `TYPE_CHECKING` import available at runtime. Frameworks that inspect annotations may fail to resolve those names. Check what annotation consumers need and keep required names importable. Use syntax supported by the project's minimum version.

## Related rules

- [Containers](type-containers.md)
