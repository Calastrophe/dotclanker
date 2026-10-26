# fn-comprehensions

> Use a comprehension when its transformation and filtering fit in one readable expression.

## Why it matters

A comprehension makes a simple mapping or filter compact. Deeply nested clauses can hide control flow and make it harder to see which values are produced.

## Guidance

Use list, set, or dictionary comprehensions for direct transformations. Use a loop when the body has multiple steps, nested branching, side effects, or several levels of iteration. A multiline comprehension is fine when each clause remains easy to scan.

## Bad

```python
pairs = [(left, right) for left in rows for right in rows if left != right and valid(left, right)]
```

## Good

```python
pairs = []
for left in rows:
    for right in rows:
        if left != right and valid(left, right):
            pairs.append((left, right))
```

## Caveats

A straightforward nested comprehension can be clear when the input is small and the relationship is obvious. Prefer readability over a fixed limit on clauses.
