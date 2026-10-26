# fn-iterators

> Use iteration and membership operations directly when a container already supports them.

## Why it matters

Built-in iteration expresses the operation without making an unnecessary copy. It also works with iterables that do not provide list-style helper methods.

## Guidance

Iterate over a mapping for its keys, use `.items()` when both keys and values are needed, and use `in` for membership. Avoid building a temporary list just to loop or search. Do not add or remove mapping keys or set elements while iterating over that same container. When a loop must change the source, take a snapshot of the keys or elements first.

## Bad

```python
for key in list(settings.keys()):
    print(key)
```

## Good

```python
for key in settings:
    print(key)
```

## Caveats

A snapshot is useful when the loop intentionally mutates its source, such as deleting selected dictionary keys. Choose the snapshot's shape deliberately; a list copy has a cost and may need to include values as well as keys.

## Related rules

- [Comprehensions](fn-comprehensions.md)
- [Generators](fn-generators.md)
