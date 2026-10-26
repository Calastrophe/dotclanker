# state-global

> Keep mutable state owned by the object or operation that changes it.

## Why it matters

Mutable module-level state hides dependencies, complicates tests, and makes behavior depend on call order or import order.

## Guidance

Pass changing values into a function or store them on an object with a clear lifetime. Keep module-level constants immutable. For caches or registries, define their lifecycle, invalidation, and concurrency behavior where they are created.

## Bad

```python
current_user = None


def render_dashboard() -> str:
    return f"Hello, {current_user.name}"
```

## Good

```python
def render_dashboard(user: User) -> str:
    return f"Hello, {user.name}"
```

## Caveats

A module-level cache, registry, or singleton can be justified when shared identity is part of the design. Make access explicit and bound its lifetime or size when appropriate.

## Related rules

- [Default arguments](state-default-args.md)
