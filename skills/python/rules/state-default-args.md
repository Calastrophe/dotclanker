# state-default-args

> Do not use mutable or time-dependent expressions as function defaults.

## Why it matters

Python evaluates a default expression once when it defines the function, so later calls can share the same object or reuse an old value.

## Guidance

Use immutable defaults when a fixed default makes sense. Use `None` or a private sentinel when each call needs a fresh value, then create that value inside the function.

## Bad

```python
def add_tag(tag: str, tags: list[str] = []) -> list[str]:
    tags.append(tag)
    return tags
```

## Good

```python
def add_tag(tag: str, tags: list[str] | None = None) -> list[str]:
    if tags is None:
        tags = []
    tags.append(tag)
    return tags
```

## Caveats

Use a unique sentinel if `None` is itself a valid argument with a distinct meaning. Immutable defaults such as `()` are fine, but immutability is shallow when a tuple contains mutable objects.
