# res-context-managers

> Give files, locks, transactions, and other acquired resources a clear cleanup boundary.

## Why it matters

A context manager releases or finalizes a resource even when the body raises or returns early.

## Guidance

Use `with` for files, locks, and objects that implement the context manager protocol. Open text files with an explicit encoding when the expected encoding is known. For resources without a context manager, use `try`/`finally` or `contextlib` to make cleanup reliable.

## Bad

```python
def read_config(path: str) -> str:
    source = open(path)
    return source.read()
```

## Good

```python
def read_config(path: str) -> str:
    with open(path, encoding="utf-8") as source:
        return source.read()
```

## Caveats

Binary files do not take a text encoding. Use a different context manager when a library provides one for sockets, transactions, temporary directories, or other resources.
