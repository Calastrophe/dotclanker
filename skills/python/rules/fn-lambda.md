# fn-lambda

> Use a lambda only for a short expression whose purpose is clear at the call site.

## Why it matters

A named function is easier to test, reuse, inspect in a traceback, and explain when the operation grows.

## Guidance

A lambda works well for a tiny callback, such as a sort key. Use a `def` for multi-step logic, a reusable operation, or behavior that needs a docstring or a meaningful name.

## Bad

```python
records.sort(key=lambda record: (record.group.lower(), -record.score, record.created_at.isoformat()))
```

## Good

```python
def record_sort_key(record: Record) -> tuple[str, float, str]:
    return record.group.lower(), -record.score, record.created_at.isoformat()


records.sort(key=record_sort_key)
```

## Caveats

Do not expand a clear one-line callback into a named function just to avoid lambda syntax. Use `operator` functions when they directly name a standard operation.
