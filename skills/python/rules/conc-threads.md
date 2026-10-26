# conc-threads

> Make shared state and worker lifetimes explicit when using threads.

## Why it matters

Threads can interleave operations. Unsynchronized shared mutation creates failures that are hard to reproduce, and unmanaged workers can outlive the operation that started them.

## Guidance

Prefer passing data to workers and returning results. Protect shared mutable state with a lock or use a queue when workers exchange messages. Use an executor or another managed worker API so tasks are joined and exceptions are observed. Do not hold a lock while waiting on unrelated blocking work.

## Bad

```python
counter = 0


def add_one() -> None:
    global counter
    counter += 1
```

## Good

```python
from threading import Lock

counter = 0
counter_lock = Lock()


def add_one() -> None:
    global counter
    with counter_lock:
        counter += 1
```

## Caveats

Threads are often useful for blocking I/O. The best choice for CPU-bound work depends on the Python runtime and workload; measure before choosing processes, interpreters, or threads.
