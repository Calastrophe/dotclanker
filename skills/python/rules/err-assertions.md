# err-assertions

> Use assertions for internal invariants, not input validation or required program behavior.

## Why it matters

Python can remove `assert` statements when optimization is enabled. A check that guards required behavior must therefore use normal control flow.

## Guidance

Use an explicit condition and raise an appropriate exception for invalid arguments or data. Use `assert` when a failure means the program has violated an internal assumption and continuing would indicate a bug. Tests can use assertions to state expected results.

## Bad

```python
def set_port(port: int) -> None:
    assert 1 <= port <= 65535
    configure_port(port)
```

## Good

```python
def set_port(port: int) -> None:
    if not 1 <= port <= 65535:
        raise ValueError(f"port out of range: {port}")
    configure_port(port)
```

## Caveats

An assertion is useful after a branch or at a private invariant boundary when the condition is not part of the caller's input contract. Do not use it for checks that must run in production.

## Related rules

- [Exceptions](err-exceptions.md)
