# doc-docstrings

> Document public behavior and non-obvious contracts where callers need to find them.

## Why it matters

A useful docstring explains how to call an object and what its results or side effects mean. Repeating the implementation adds noise and can go stale.

## Guidance

Write a concise summary for public modules, classes, and functions when their purpose or contract is not obvious. Document meaningful arguments, return values, exceptions, side effects, and resource ownership. Use the repository's established docstring format. Use comments to explain a non-obvious reason or constraint, not to narrate the next line.

## Bad

```python
from collections.abc import Sequence


def total(prices: Sequence[float]) -> float:
    """Loops through the prices and adds them together."""
    result = 0.0
    for price in prices:
        result += price
    return result
```

## Good

```python
from collections.abc import Sequence


def total(prices: Sequence[float]) -> float:
    """Return the sum of prices before tax."""
    result = 0.0
    for price in prices:
        result += price
    return result
```

## Caveats

Internal helpers do not need boilerplate docstrings when their names and types make the contract clear. Keep any docstring format consistent with project tooling.
