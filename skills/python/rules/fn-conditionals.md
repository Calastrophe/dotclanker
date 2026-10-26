# fn-conditionals

> Keep conditional expressions and boolean tests explicit about what they distinguish.

## Why it matters

Compact conditions are easy to misread when several values have special meanings or a conditional expression grows beyond a simple choice.

## Guidance

Use a conditional expression for a short, direct choice. Use a statement for multiple branches or multi-step values. Use truthiness for ordinary empty-container checks, but compare with `None` using `is` and distinguish `False` from `None` when the distinction matters.

## Bad

```python
def selected(items: list[str] | None) -> list[str]:
    return items or ["default"]
```

## Good

```python
def selected(items: list[str] | None) -> list[str]:
    if items is None:
        return ["default"]
    return items
```

## Caveats

Some array and domain types reject truth-value checks because they have no single truth value. Use their documented emptiness or validity operation.
