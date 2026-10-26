# class-decorators

> Add a decorator when its transformation or policy is clearer than the repetition it removes.

## Why it matters

Decorators run when a function or class is defined and can change the callable that users see. Hidden behavior can complicate imports, signatures, debugging, and tests.

## Guidance

Keep decorators focused, document non-obvious effects, and preserve wrapped function metadata with `functools.wraps`. Avoid doing I/O or connecting to external services while a decorator is applied. Use standard decorators such as `property` or `classmethod` when their contracts fit.

## Bad

```python
import functools


def traced(function):
    def wrapper(*args, **kwargs):
        print(function.__name__)
        return function(*args, **kwargs)
    return wrapper
```

## Good

```python
import functools


def traced(function):
    @functools.wraps(function)
    def wrapper(*args, **kwargs):
        print(function.__name__)
        return function(*args, **kwargs)
    return wrapper
```

## Caveats

Frameworks may require decorators with particular signatures or import-time registration. Follow those contracts and keep registration effects limited and explicit.
