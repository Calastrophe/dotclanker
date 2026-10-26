# class-super

> Use `super()` to call the next implementation in the method resolution order.

## Why it matters

Cooperative inheritance lets each class in a hierarchy contribute its behavior once, including in multiple inheritance. Calling a named base class directly can bypass another class in that order.

## Guidance

Use zero-argument `super()` in ordinary instance methods. In multiple inheritance, ensure each participating method calls `super()` with compatible arguments. Override only when the subclass needs to change behavior, and keep the base contract intact.

## Bad

```python
class CachedReport(Report):
    def render(self) -> str:
        return Report.render(self).replace("draft", "cached")
```

## Good

```python
class CachedReport(Report):
    def render(self) -> str:
        return super().render().replace("draft", "cached")
```

## Caveats

A non-cooperative third-party base class may require a direct call as an adapter. Explain that boundary rather than mixing direct calls into a cooperative hierarchy.
