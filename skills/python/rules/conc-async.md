# conc-async

> Keep blocking work off the event loop and make task lifetimes explicit.

## Why it matters

A blocking call inside an async function pauses every task sharing that event loop. Detached tasks can also fail after the caller has stopped observing them.

## Guidance

Use `await` with asynchronous APIs. Keep CPU-heavy or blocking calls outside the event loop. For blocking I/O without an async client, `asyncio.to_thread()` can run the synchronous operation in a worker thread.

Await tasks whose results or failures matter. Choose a task group or another concurrency pattern that matches the required failure and cancellation behavior.

## Bad

```python
from urllib.request import urlopen


async def load_page(url: str) -> str:
    with urlopen(url, timeout=10) as response:
        return response.read().decode("utf-8")
```

## Good

```python
import asyncio
from urllib.request import urlopen


def _read_page(url: str) -> str:
    with urlopen(url, timeout=10) as response:
        return response.read().decode("utf-8")


async def load_page(url: str) -> str:
    return await asyncio.to_thread(_read_page, url)
```

## Caveats

`asyncio.to_thread()` is available from Python 3.9 and is intended mainly for blocking I/O. Cancelling the coroutine that awaits it does not stop a synchronous function already running in its thread. Give the blocking client its own timeout and account for work that may continue after the caller times out.

The example's `urlopen` timeout bounds blocking socket operations rather than the entire request.

For multiple tasks, default `asyncio.gather()` propagates the first exception while leaving other awaitables running. `TaskGroup`, available from Python 3.11, cancels remaining tasks after a child raises an exception other than `CancelledError` and waits for them.

Choose the behavior your operation needs.

Use a synchronous API at a synchronous boundary when no event loop is active. Do not call `asyncio.run()` from code already running inside an event loop.

## Related rules

- [Threads](conc-threads.md)
- [Context managers](res-context-managers.md)
