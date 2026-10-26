# err-exceptions

> Raise and catch exceptions at the layer that can explain or handle the failure.

## Why it matters

A useful exception preserves the failure reason and lets callers decide whether to retry, report, or recover.

## Guidance

Raise a specific built-in exception when it describes the problem. Define a project exception when callers need to distinguish a domain failure. Catch only exceptions the current layer can handle, and keep the `try` block close to the operation that may fail. When translating an exception, chain it with `raise NewError(...) from error` so the cause remains available.

## Bad

```python
try:
    user = load_user(user_id)
    return format_user(user)
except Exception:
    return None
```

## Good

```python
try:
    user = load_user(user_id)
except UserNotFoundError:
    return None

return format_user(user)
```

## Caveats

A process boundary may need a broad exception handler to log a failure and shut down cleanly. Catching broadly is also appropriate when code immediately re-raises or deliberately isolates a worker. Keep those handlers at clear boundaries and preserve traceback details.

## Related rules

- [Assertions](err-assertions.md)
